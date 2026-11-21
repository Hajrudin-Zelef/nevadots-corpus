---
id: collect-261121-veille-ia/qwen/qwen-3-8-all-6-models-benchmarks-pricing-and-local-setup
title: "Qwen 3.8: All 6 Models, Benchmarks, Pricing, and Local Setup"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-08-03"]
keywords: ["model", "qwen3", "qwen", "flash-next", "models", "alibaba", "launch", "only"]
source: https://www.lindy.ai/blog/qwen-3-8
source_anchor: ""
source_lines: [1, 262]
section: "Qwen"
---
Qwen 3.8 is a confusing thing to google. Alibaba put the name on six different models, and they range from a 27B model that'll run on a decent laptop to a 2.4-trillion-parameter flagship that needs a datacenter.

I read through Qwen's launch posts, the model cards on Hugging Face, and Alibaba's price list (prices change depending on the region you sign up in). For the stuff you only learn by running it, I leaned on Simon Willison's hands-on testing of the 27B.

Below is what each model is for, how they compare to Claude and GPT, what they cost, and how to get one running on your own machine.

## What is Qwen 3.8?

**Qwen 3.8 is the August-September 2026 generation of Alibaba's Qwen models,** and it's a family of six. The flagship, Qwen3.8-Max, is the first Max-class model Qwen has released with open weights, and the rest of the family scales down from there to a model that fits on a laptop.

Think of it like a car lineup with one badge on every model. Max and the 27B are built on the Qwen 3.5 architecture, while Flash-Next previews the engine Qwen plans to use for Qwen4.

| Model | What it is | Open weights? | Released | 
|---|---|---|---|
| Qwen3.8-Max | Flagship, 2.4T total / 95B active | Yes, as Qwen3.8-2.4T-A95B | Aug 3, 2026 (API) | 
| Qwen3.8-27B | Dense vision-language model | Yes, Apache 2.0 | Aug 14, 2026 | 
| Qwen3.8-Flash-Next | 125B total / 6B active MoE | Yes, community license | Aug 26, 2026 | 
| qwen3.8-flash | Hosted version of Flash-Next | No (API only) | Aug 26, 2026 | 
| Qwen3.8-Omni-Flash | Text, image, audio, and video in | No (API only) | Sept 18, 2026 | 
| Qwen3.8-LiveTranslate | Real-time speech interpretation | No (API only) | Sept 18, 2026 | 

The Max launch post promised open weights "next week," and they landed on Hugging Face as Qwen3.8-2.4T-A95B, a text-only build that always thinks before it answers.

The hosted Max is the fuller product. Its model card says the API version adds vision input, a non-thinking mode, and a 1M-token context window by default.

Qwen3.8-27B reads images and video, and it's the one most people mean when they say they're running Qwen 3.8 at home. It dropped on Friday, August 14, per Simon Willison's hands-on review.

Qwen3.8-Omni-Flash takes text, image, audio, and video input with a 1M-token context window, handles up to an hour of audio-visual input in one go, and comes with a Realtime variant for live conversations.

Qwen3.8-LiveTranslate is the specialist of the family, a simultaneous-interpretation model that separates speakers in a conversation and takes audio in 60 languages.

### Don't mix up Qwen3.8 and Qwen3-8B

Qwen3-8B is an older, much smaller model from the Qwen3 generation, and it's still on sale as its own line item in Alibaba Cloud Model Studio. The names sit one dot and one dash apart, so double-check the model ID before you paste it into anything.

It gets even more tangled because Qwen's own Max launch post fine-tunes a Qwen3-8B model as part of a research demo, so a tutorial that mentions "Qwen3-8B" is talking about that older model.

### Is qwen3.8-flash the same model as Flash-Next?

**qwen3.8-flash is the hosted version of Flash-Next, so the two share a base model.** The Flash-Next model card says the hosted version adds production features like a 1M-token context by default and built-in tools.

If you download Flash-Next instead, you get the 262,144-token native context and whatever tooling you bring yourself.

## What's new in Qwen 3.8

**The headline change is stamina.** Qwen pitched Qwen3.8-Max around long, unsupervised jobs, and the demos in the Max launch post run for days: one autonomous coding run on a project called oh-my-cli had racked up 265 commits, 127 pull requests, and 151 issues after about 16 days.

In another demo, Qwen entered Max in a real Tianchi competition, where it worked on its own and beat 458 of the 526 human teams within a 24-hour limit. Vendor demos are vendor demos (nobody posts the runs that went sideways), but they tell you what Qwen optimized for.

Four changes you'll feel as a user:

- **Reasoning effort you can dial:** the Max, 27B, and Flash-Next model cards all document a reasoning_effort setting with three levels, xhigh (the default), medium, and low.
- **Preserved thinking:** preserve_thinking is on by default, so the model keeps its earlier reasoning in context across a conversation, which helps agents stay consistent.
- **Long context:** the open models handle 262,144 tokens natively and stretch to about 1M tokens, per the model cards.
- **Multi-token prediction:** the models are trained with MTP, a trick that lets inference servers guess several tokens ahead and verify them, which speeds up generation.

Qwen calls Flash-Next an early preview of the architecture that will power Qwen4, pairing Gated DeltaNet layers with a new Qwen Sparse Attention design. Its launch post says training cost about a ninth of Qwen3.7-Plus.

## Qwen 3.8 benchmarks

**Qwen's own numbers put Qwen3.8-Max in the same league as the top closed models,** trading wins with Claude Opus 4.8 and GPT-5.6 Sol and trailing Claude Fable 5 on most coding tests. Here's a slice of the full table from the launch post (higher is better):

| Benchmark | Opus 4.8 | Fable 5 | GPT-5.6 Sol (max) | Qwen3.8-Max | 
|---|---|---|---|---|
| Terminal Bench 2.1 | 84.6 | 84.6 | 88.8 | 86.6 | 
| SWE-bench Pro | 69.2 | 80.0 | 64.6 | 67.7 | 
| FrontierSWE | 70.0 | 88.8 | n/a | 73.5 | 
| PaperBench | 80.3 | 88.8 | 90.5 | 93.0 | 
| CoWorkBench | 72.3 | 75.9 | 71.5 | 74.8 | 
| Toolathlon Verified | 76.2 | 77.9 | 74.9 | 72.5 | 
| GPQA Diamond | 92.0 | 92.6 | 94.1 | 92.6 | 
| HLE (no tools) | 45.7 | 53.3 | 47.2 | 43.6 | 
| IFBench | 62.2 | 63.5 | 72.7 | 82.8 | 

Against Qwen's previous flagship, the jump is large. On the DeepSWE 1.1 coding benchmark, Qwen3.7-Max scored 21.6 and Qwen3.8-Max scored 56.6, though that still trails all three rivals on the same row (Opus 4.8 at 59.0, Fable 5 at 70.0, and GPT-5.6 Sol at 73.0).

The smaller models hold up well too. The Flash-Next model card compares both open mid-size models against Claude Opus 4.6 (Max):

| Benchmark | Flash-Next | 27B | Qwen3.7-Plus | Opus 4.6 (Max) | 
|---|---|---|---|---|
| SWE-bench Pro | 62.5 | 61.7 | 55.8 | 53.4 | 
| CoWorkBench | 73.9 | 70.7 | 65.1 | 68.2 | 
| GPQA Diamond | 91.7 | 89.2 | 90.3 | 91.3 | 
| LiveCodeBench v6 | 91.9 | 90.3 | 89.6 | 88.8 | 

### How much to trust these numbers

Every score above comes from Qwen's own launch post and model cards. Qwen ran most of its coding tests itself (many of them inside Claude Code), a few benchmarks are in-house tests like CoWorkBench, and a footnote warns that the Fable 5 results "may involve fallbacks."

Qwen's own cards don't always agree, either. The 27B card lists Qwen3.7-Plus at 57.6 on SWE-bench Pro, while the Flash-Next card lists it at 55.8. Small wobble, but it's a reminder to wait for independent evals before you bet a roadmap on a two-point lead.

### Is Qwen 3.8 better than Claude?

**Qwen3.8-Max beats Claude Opus 4.8 on some of Qwen's tests and loses on others.** Max leads on Terminal Bench 2.1 (86.6 vs 84.6), FrontierSWE (73.5 vs 70.0), PaperBench (93.0 vs 80.3), and instruction following (82.8 vs 62.2 on IFBench).

Opus 4.8 stays ahead on SWE-bench Pro (69.2 vs 67.7), Toolathlon Verified (76.2 vs 72.5), and HLE (45.7 vs 43.6). Against Claude Fable 5, Max trails on most coding rows in Qwen's full table, sometimes by a wide margin (80.0 vs 67.7 on SWE-bench Pro). Qwen's table predates Claude Fable 5.2, which Anthropic released on November 9, 2026.

So "better" depends on the job and the budget. If you're weighing Qwen alongside other alternatives to Claude or OpenAI's new GPT-6 family, the price difference below is where Qwen makes its strongest case.

## Qwen 3.8 pricing

**Qwen 3.8 API prices depend on which Alibaba Cloud region you deploy in,** and Singapore runs roughly 20% to 33% higher than US (Virginia). These are standard pay-as-you-go prices per 1 million tokens from Model Studio's pricing page, last updated October 3, 2026:

| Model | Singapore (input / output) | US Virginia (input / output) | 
|---|---|---|
| qwen3.8-max | $2 / $6 | $1.65 / $4.951 | 
| qwen3.8-flash | $0.15 / $0.47 | $0.113 / $0.382 | 
| qwen3.8-omni-flash | $0.15 / $0.47 | $0.113 / $0.382 | 
| qwen3.8-27b (hosted) | $0.50 / $3 | not listed | 

LiveTranslate is billed by what goes in and out. In Singapore, qwen3.8-livetranslate-flash-realtime costs $7.50 per million audio-input tokens, $20 per million text-output tokens, and $30 per million audio-output tokens.

The US (Virginia) prices match Frankfurt, Tokyo, Hong Kong, and Beijing for Max and Flash. Singapore costs more but is the only region with a free quota: 1 million tokens each for Max, Flash, LiveTranslate, and the hosted 27B, valid for 90 days. Omni-Flash isn't included.

For comparison, Claude Opus 4.8 costs $5 per million input tokens and $25 per million output tokens, so Max in Singapore runs at less than half the input price and about a quarter of the output price. DeepSeek's budget model is cheaper still per token; our DeepSeek V4 Flash pricing breakdown has its current rates.

On qwen3.8-max, the output price covers the chain of thought plus the answer, so every reasoning token Max spends at xhigh lands on your bill at the output rate.

Context caching also changes the math. Cache-hit tokens are billed below the standard input price (the exact rate for each Qwen 3.8 model is listed in Model Studio), which adds up fast for agents that resend the same long context every turn.

Moving an existing app over to save money follows much the same playbook as switching from Claude to DeepSeek. Qwen's APIs speak both the OpenAI and Anthropic formats, so the technical swap is mostly a base URL and a model name, and the real work is re-testing your prompts.

### What you can use for free

Three parts of Qwen 3.8 cost nothing up front:

1. **The open weights:** Qwen3.8-27B, Flash-Next, and the Max weights are free to download from Hugging Face. You pay for the hardware (or cloud GPUs) to run them.
2. **The Qwen chat app:** Qwen's site describes its assistant as "free to use," and the Max model card links straight to Qwen Studio with qwen3.8-max selected.
3. **The Singapore trial quota:** 1 million free tokens each for Max, Flash, LiveTranslate, and the hosted 27B for 90 days, only in the Singapore region.

### Is Qwen 3.8 open source?

**Qwen 3.8 is partly open source, and the license depends on the model:**

1. **Qwen3.8-27B:** Apache 2.0, the least restrictive license in the family.
2. **Qwen3.8-2.4T-A95B (Max weights):** a custom Qwen3.8-Max License. Anyone running a model-as-a-service or AI work assistant business with more than $50 million in revenue over 12 months needs a separate license from Qwen.
3. **Qwen3.8-Flash-Next:** the Qwen Community License 1.0, which requires a separate license from any company that runs a model-as-a-service or AI work assistant business before it uses Flash-Next commercially, with no revenue threshold.

Both custom licenses also ask very large products (100 million monthly users or $20 million in monthly revenue) to display the model name in their interface. Internal use is exempt, so most teams are fine, but read the license before you build a product on the Max or Flash-Next weights.



## How to run Qwen 3.8 locally

**The 27B is the Qwen 3.8 model you can realistically run at home.** The others need workstation or datacenter hardware, and the download sizes tell you most of what you need to know:

| Model build | Download size | Where it fits | 
|---|---|---|
| 27B, Ollama Q4_K_M | 18GB | Well-specced laptop | 
| 27B, Ollama Q8_0 | 30GB | High-memory desktop or Mac | 
| 27B, Ollama BF16 | 56GB | GPU workstation | 
| Flash-Next, Ollama MLX | 105GB | Large-memory Mac | 
| Flash-Next, Ollama Q4_K_M | 120GB | Multi-GPU server | 
| Max, FP8 weights | About 2.5TB | Datacenter | 

Rule of thumb: you need at least as much memory as the file, plus headroom for the context window.

Qwen also publishes FP8 builds of all three open models on Hugging Face, like Qwen3.8-27B-FP8, which shrink the footprint on GPUs that support FP8. Simon Willison ran the 27B on a 128GB M5 Max MacBook Pro and an NVIDIA DGX Spark using a 17GB Q4_K_M build in LM Studio.

Flash-Next's full checkpoint is about 180B parameters, because 51 billion n-gram embedding parameters sit on top of the 125B model. On smaller rigs, those embeddings can live in host memory and get prefetched as needed, so they don't occupy GPU memory full time.

Here's the shortest path to a working setup:

1. **Install a local runner:** Ollama (which also has MLX builds for Apple silicon) or LM Studio, which Simon used for his tests.
2. **Pull the model:** ollama run qwen3.8 grabs the 27B at Q4_K_M (18GB, 256K context, text and image input).
3. **Raise the context limit:** LM Studio's default 8,192-token context filled up with reasoning on even simple prompts in Simon's tests. He loaded the full 262,144 tokens and the problem went away, so raise it as high as your memory allows.
4. **Turn reasoning down:** start with reasoning_effort set to low, or switch thinking off per request (the 27B supports that), then raise it only for hard tasks.
5. **Speed it up with MTP:** in llama.cpp, the model's built-in multi-token prediction ran about 72% faster than the default LM Studio build in Simon's comparison on the DGX Spark.
6. **Serve it properly for a team:** for production, Qwen recommends SGLang, vLLM, or TokenSpeed, and suggests temperature=1.0, top_p=0.95, and top_k=20 for thinking mode.

The Max weights belong to teams with serious GPU clusters, since the FP8 files alone add up to roughly 2.5TB.

### Or skip the hardware and use the API

If you want Max-level quality without a server room, the API is the easier route. Qwen's APIs accept both the Anthropic and OpenAI Responses formats, so you can set qwen3.8-max as the model inside Claude Code or Codex and point your favorite AI coding agent at Qwen.

For Claude Code, Alibaba's setup guide puts the connection details in ~/.claude/settings.json. This example uses the Singapore endpoint:

{

"env": {

"ANTHROPIC_AUTH_TOKEN": "YOUR_API_KEY",

"ANTHROPIC_BASE_URL": "https://{WorkspaceId}.ap-southeast-1.maas.aliyuncs.com/apps/anthropic",

"ANTHROPIC_MODEL": "qwen3.8-max"

}

}

Codex works the same way through a small config file. Your API key only works in the region where you created it, and that region also decides your price.

## Where Qwen 3.8 falls short

**The biggest complaint is overthinking.** At the default xhigh effort, the 27B treats every request like a thesis defense, and Simon Willison's write-up has the most detailed evidence so far.

He asked it to "draw an svg of a circle." Its reasoning trace started planning concentric guide rings, tick marks, and a slowly rotating dashed ring, and several minutes later it delivered an animated circle that looked great and was far more than he asked for.

His pelican-on-a-bicycle test took 21 minutes and 22,276 reasoning tokens to produce 3,223 tokens of output. With reasoning off, the same prompt took 137 seconds. His advice: ignore the default and start at low or no reasoning.

A few other weak spots worth knowing before you commit:

- **Token bills:** on the qwen3.8-max API, all that reasoning is billed at the output rate, so leaving xhigh on for simple jobs means paying for thousands of tokens you never read.
- **Local speed:** Simon saw about 15 to 30 tokens per second in LM Studio. Dense models like the 27B need plenty of memory bandwidth, and neither of Simon's machines is a top performer on that front.
- **The open Max is limited:** the downloadable Max weights are text-only, and thinking can't be disabled, so the vision and non-thinking features live only in the hosted API.
- **Omni-Flash and LiveTranslate are API-only:** there's no open-weight version of either audio model on Hugging Face.
- **Self-reported scores:** independent benchmarks are still catching up, and Qwen ran most of the tests itself.

Most of these shrink once you set it up right, and a sensible reasoning setting fixes the worst of them.

## Picking the right Qwen 3.8 model for your work

Qwen 3.8 is six models with one naming scheme, so the right pick comes down to where you want it to run and what you're paying for:

- **Private, on your own machine:** Qwen3.8-27B through Ollama or LM Studio, with reasoning turned down.
- **Cheap, fast API calls:** qwen3.8-flash at $0.15 in and $0.47 out per million tokens in Singapore, or less in US Virginia (worth pricing against Gemini 3.8 Flash before you commit).
- **The hardest coding and agent work:** qwen3.8-max through the API, and budget for reasoning tokens.
- **Meetings, audio, and video:** Qwen3.8-Omni-Flash, which takes up to an hour of audio-visual input in one go.
- **Live interpretation:** Qwen3.8-LiveTranslate, which tells speakers apart and translates as they talk.
- **Self-hosting at scale:** Flash-Next, if you have the GPUs and your use fits its community license.

If you'd like the work handled inside Slack, Lindy is an AI teammate that lives in your company's workspace and takes requests right in the conversation. Whichever route you pick, set reasoning_effort on day one, because the default xhigh setting is where most of the wasted time and tokens come from.



## Frequently asked questions

### Is Qwen 3.8 Max completely free?

**Qwen 3.8 Max is free to download but paid to use through the API.** The Qwen3.8-2.4T-A95B weights cost nothing under the Qwen3.8-Max License, while the qwen3.8-max API costs $2 per million input tokens and $6 per million output tokens in Singapore, where a 1 million-token free quota lasts 90 days.

### Is Qwen 3.8 any good?

**Qwen 3.8 is very good on Qwen's own benchmarks and in early hands-on tests.** Max trades wins with Claude Opus 4.8 and GPT-5.6 Sol, and the 27B drives coding agents and handles vision tasks on high-end consumer hardware. Its main weakness is overthinking at the default xhigh setting.

### Is Qwen 3.8 better than Claude?

**Qwen 3.8 Max beats Claude Opus 4.8 on some benchmarks and trails it on others.** It leads on Terminal Bench 2.1 and IFBench but falls behind on SWE-bench Pro and Toolathlon Verified, and Claude Fable 5 outscores it on most coding tests in Qwen's full table. Qwen's main edge is price.

### Is Qwen AI free?

**Qwen AI is free to use as a chat assistant,** according to Qwen's website, which puts it among the free ChatGPT-style assistants you can try today. The Qwen API is pay-as-you-go, and the open-weight models are free to download.

### Can you turn off thinking in Qwen 3.8?

**Yes on the 27B and the hosted Max, but not on the open Max weights.** The 27B's thinking mode can be switched off per request, and the hosted qwen3.8-max adds a non-thinking mode. The downloadable Qwen3.8-2.4T-A95B always thinks before it answers.

### Can I use Qwen 3.8 commercially?

**Yes, with limits on two of the three open models.** Qwen3.8-27B is Apache 2.0. The Max weights need a separate license only for model-as-a-service or AI work assistant businesses with over $50 million in revenue, and Flash-Next needs one for any company in those businesses.

### Can I run Qwen 3.8 on a laptop?

**You can run Qwen3.8-27B on a well-specced laptop.** The Ollama Q4_K_M build is an 18GB download, and Simon Willison ran a 17GB build in LM Studio on a 128GB MacBook Pro and a DGX Spark at about 15 to 30 tokens per second. Max and Flash-Next need workstation or datacenter hardware.

## About the editorial team

**Flo Crivello**Founder and CEO of Lindy

Flo Crivello is the founder and CEO of Lindy. Before that, he founded Teamflow and was a product manager at Uber. He writes about technology, startups, and the future of work on his blog.
