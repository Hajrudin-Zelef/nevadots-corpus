---
id: collect-261121-veille-ia/deepseek/deepseek-v4-pro-pricing-benchmarks-and-self-hosting
title: "DeepSeek V4 Pro: Pricing, Benchmarks and Self-Hosting"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-04-24"]
keywords: ["model", "deepseek", "preview", "card", "v4-pro", "models", "pricing", "under"]
source: https://www.truefoundry.com/blog/eepseek-v4-pro
source_anchor: ""
source_lines: [1, 133]
section: "DeepSeek"
---
# DeepSeek V4-Pro Is GA: What an MIT-Licensed Frontier Model Actually Changes

### Built for Speed: ~10ms Latency, Even Under Load

Blazingly fast way to build, track and deploy your models!

- Handles 350+ RPS on just 1 vCPU — no tuning needed
- Production-ready with full enterprise support

## **What DeepSeek shipped**

DeepSeek-V4-Pro entered preview on 24 April 2026 and went generally available on 13 August 2026 — just under four months. The API model name never changed: you still call deepseek-v4-pro, and the GA build simply replaced the preview behind it.

The specifications, from DeepSeek’s Models & Pricing page and the GA model card, as of September 2026:

Two details matter more than the parameter count.

First, **the weights are genuinely open**. The GA checkpoint is published at deepseek-ai/DeepSeek-V4-Pro-0813 under the MIT licence — not a bespoke community licence with a revenue ceiling or a field-of-use carve-out. MIT is about as permissive as it gets. That is the single fact that makes the rest of this post worth writing.

Second, **the efficiency work is the architecture story**. The preview model card describes a hybrid attention design combining Compressed Sparse Attention and Heavily Compressed Attention, and reports that at a 1M-token context V4-Pro needs roughly 27% of the single-token inference FLOPs and 10% of the KV cache of DeepSeek-V3.2. Long context is cheap to serve here in a way it was not in the V3 generation, which is precisely what makes self-hosting arithmetic interesting.

[SCREENSHOT: DeepSeek chat — the Expert Mode selector that routes to V4-Pro on app and web]

## **What changed versus the preview**

DeepSeek is unusually direct about this. The GA model card states that V4-Pro-0813 “is built on the DeepSeek-V4-Pro (Preview) model structure, with a DSpark speculative decoding module attached” — the base model is the same; the post-training and the decoding path are not.

The deltas it publishes, preview to GA:

Source: DeepSeek-V4-Pro-0813 model card, September 2026.

A word of caution on reading these. DeepSeek notes that code-agent scores were produced using its own DeepSeek Harness in minimal mode at max reasoning effort, temperature = 1.0, top_p = 0.95. A near-fivefold jump on DeepSWE is a post-training result measured under the vendor’s own harness, not an independent reproduction. The direction is clearly real; the magnitude deserves your own eval.

Alongside the model, GA brought native OpenAI Responses API support with a one-click Codex configuration, and the three-level reasoning_effort control that replaces the binary thinking toggle.

## **The benchmarks, read honestly**

The GA model card publishes a head-to-head table against both open and closed competitors. Here is the part everyone reproduces, and then the part most posts leave out.

Source: DeepSeek-V4-Pro-0813 model card, September 2026.

**Where V4-Pro leads.** Terminal-style agentic work and security-flavoured coding. It clears Opus-4.8 on Terminal Bench 2.1, DeepSWE, Cybergym and AutomationBench. For a model you can download, that is a genuine result.

**Where the closed models still win.** Raw knowledge and the harder repository work. Opus-4.8 is ahead on NL2Repo by more than eight points, on DSBench-Hard, on Toolathlon-Verified and on HLE without tools. Fable-5 with fallback leads on most of the same. If your workload is dominated by hard multi-file repository changes or by breadth-of-knowledge questions, the frontier closed models remain the better answer, and no amount of licence enthusiasm changes that.

**Where the other open models sit.** Kimi K3 edges V4-Pro on Terminal Bench 2.1 and DeepSWE. “Best open-weight model” is contested, not settled. The interesting claim is narrower and more durable: a downloadable model is now close enough to the frontier that the deployment question is live.

## **What DeepSeek V4 pricing actually looks like**

DeepSeek introduced peak and off-peak billing alongside GA, effective 16:00 UTC on 16 August 2026. Off-peak rates are exactly half of peak. Peak hours are 01:00-04:00 and 06:00-10:00 UTC, Monday to Friday, excluding Chinese public holidays; everything else, weekends included, is off-peak.

Source: DeepSeek Models & Pricing, September 2026.

Three things to notice. The cache-hit price is 30x below cache-miss, so prompt structure matters enormously for anything with a stable system prompt. The peak/off-peak split is a scheduling lever, not a discount — batch work moved to off-peak halves its bill. And DeepSeek raised prices at GA relative to the preview, though the preview’s published rates are [VERIFY] — they are no longer on the pricing page and we will not source them from secondary coverage.

One operational note: the API concurrency limit for deepseek-v4-pro is 500. DeepSeek also confirmed on 10 September 2026 that it will continue serving the V4 Pro API beyond 14 September 2026 with billing unchanged (change log).

[SCREENSHOT: DeepSeek Platform — usage dashboard showing peak vs off-peak token spend]

## **Where teams hit trouble**

Four failure modes come up repeatedly with open-weight frontier models, and V4-Pro has all four.

**The self-hosting bill is a node, not a GPU.** DeepSeek’s reference serving command in the model card targets a single 4-node GB300 configuration via vLLM, with FP8 KV cache, expert parallelism and DSpark speculative decoding enabled. A 1.7T-parameter MoE is not something you spin up on a spare A100. The open weights give you the *option* to self-host; whether the arithmetic beats $1.98 per million output tokens depends entirely on your utilisation. Below a fairly high steady-state load, the API wins.

**There is no chat template.** The GA release ships no Jinja chat template. DeepSeek instead provides an encoding folder of Python scripts for turning OpenAI-format messages into model input and parsing the output back. Anything in your stack that assumes tokenizer.apply_chat_template will need work.

**V4-Pro has no vision.** Vision lives on the Flash line, not Pro. A single-model strategy that assumed multimodality breaks here, which means you are routing across at least two models whether you planned to or not.

**Residency and API are now separate decisions.** Sending prompts to api.deepseek.com is a different data-flow question from running the same weights inside your own VPC. Teams frequently conflate “we can use DeepSeek V4” with “we can call the DeepSeek API”, and those get different answers from the same security review. Your own counsel decides what is acceptable; the point is only that the open weights make both paths technically available.

## **Where TrueFoundry fits**

An open-weight frontier model only changes your economics if you can actually put it somewhere. That is the part TrueFoundry does.

**Self-host it in your own VPC.** The AI Gateway registers self-hosted open-weight models alongside hosted providers: you deploy V4-Pro on your own Kubernetes with vLLM or SGLang, point the gateway at the endpoint, and it appears in the model catalogue with the same routing, rate limiting, cost tracking and observability as any API model (self-hosted models docs). Because vLLM and SGLang both serve an OpenAI-compatible API by default, the gateway does not need a DeepSeek-specific adapter.

**Or call the API and keep the option open.** If you are not ready to self-host, route to DeepSeek’s API through the gateway and keep the same call path you would use later for a self-hosted deployment. Switching from the hosted endpoint to your own cluster becomes a config change rather than an application rewrite.

**Route by complexity, not by habit.** The benchmark table above is an argument for a mixed fleet, not a migration, and cost- and quality-aware routing is how you run one. Hard repository work to Opus-4.8, terminal-shaped agent runs to V4-Pro, classification and extraction to something cheaper. In TrueFoundry’s own Auto Routing benchmark across 550 prompts, tiering requests cut cost 69% while retaining 98% of baseline quality, with mean latency dropping from 7.6s to 4.0s; on production-shaped traffic the reduction reached 80%.

**Apply the same guardrails either way.** A self-hosted model does not inherit a provider’s safety stack. Gateway-level guardrails run on the request and response path regardless of whether the model is DeepSeek’s endpoint or your own pod.

**Measure before you commit.** Per-request cost and latency per team is the only way to find out whether V4-Pro’s off-peak pricing or a self-hosted node actually beats what you run today.

## **How to run DeepSeek V4-Pro through the gateway**

The short version, as of September 2026:

1. **Decide hosted or self-hosted.** Hosted is api.deepseek.com, OpenAI or Anthropic format. Self-hosted is vLLM or SGLang on your own Kubernetes; DeepSeek publishes launch commands for both, and DSpark speculative decoding is a single flag (--speculative-config with method: dspark on vLLM, --speculative-algorithm DSPARK on SGLang).
2. **Register the model.** For a self-hosted deployment, add it under AI Gateway > Models > Self Hosted Models with the endpoint URL, model ID, server type and auth (docs). For the hosted API, add DeepSeek as a provider account.
3. **Set the reasoning effort deliberately.** low for simple calls, high for everyday agent work, max for the hard cases. This is the biggest single cost lever on the model, because max is what produces both the headline benchmarks and the headline token bills. DeepSeek recommends a 384K max output length at high and max.
4. **Route, do not migrate.** Send a slice of real traffic, compare against your incumbent on your own tasks, and expand only where it wins.

## **Related reading**

## **Conclusion**

DeepSeek V4-Pro’s GA release is not a benchmark story. On the numbers DeepSeek itself publishes, it beats Opus-4.8 on some agentic tasks and loses to it on harder repository work and on knowledge, and Kimi K3 contests its lead among open models. Taken as a leaderboard entry, it is a good model with an uneven scorecard.

Taken as a licence, it is something else. A 1M-context model close enough to the frontier to be a real option, published under MIT, means the deployment question stops being rhetorical. You can run it in your own VPC, keep prompts on infrastructure you control, and pay for compute instead of tokens — if your utilisation justifies a serious node, and if you are prepared to own the serving stack. For most teams today it will not, and the right move is to route to the API while keeping the self-hosted path open.

That is the honest close: this comes down to what you want to own. If you want someone else to run the model, the closed frontier is still the shortest path and still the best on the hardest work. If you want the option to move the model inside your perimeter without rewriting the application, an open-weight model behind a gateway is how you buy that option cheaply — and you should buy it before you need it.

TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.

## Recent Blogs

## Frequently asked questions

### What is DeepSeek V4 and when was it released?

DeepSeek V4 is DeepSeek’s mixture-of-experts model family. The V4 preview, covering V4-Pro and V4-Flash, launched on 24 April 2026. DeepSeek-V4-Pro reached general availability as DeepSeek-V4-Pro-0813 on 13 August 2026.

### Is DeepSeek V4-Pro open source?

The weights and repository are published under the MIT licence on Hugging Face at deepseek-ai/DeepSeek-V4-Pro-0813. That covers the weights and code, not the training data, so “open weight” is the more precise term than “open source”.

### How much does the DeepSeek V4 API cost?

As of September 2026: $0.66 per million input tokens off-peak and $1.32 at peak on a cache miss, $0.022 and $0.044 on a cache hit, and $1.98 and $3.96 per million output tokens. Peak hours are 01:00-04:00 and 06:00-10:00 UTC on weekdays.

### Can I self-host DeepSeek V4-Pro?

Yes, and DeepSeek publishes vLLM and SGLang serving recipes. Be realistic about scale: it is a 1.7T-parameter model and the reference configuration in the model card is a multi-GPU GB300 node.

### Does an AI gateway add meaningful latency?

TrueFoundry’s AI Gateway adds roughly 3-4 ms and sustains 350+ RPS on 1 vCPU, which is immaterial next to multi-second generation times.

### Do I have to pick one model?

No, and the benchmark spread above is the argument against it. Routing by task type across open and closed models is usually cheaper and better than standardising on one.
