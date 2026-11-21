---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-pricing-specs-open-weights-api
title: "MiniMax M3.1 Flash: Pricing, Specs, Open Weights, API"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["flash", "minimax", "input", "minimax-m3", "tokens", "model", "openrouter", "weights"]
source: https://codersera.com/blog/minimax-m3-1-flash-complete-guide-2026/
source_anchor: ""
source_lines: [1, 269]
section: "MiniMax"
---
**Quick answer.**MiniMax M3.1 Flash became generally available on 19 October 2026. In the API it is

`MiniMax-M3.1-Flash`, and on OpenRouter it is `minimax/minimax-m3.1-flash`. It costs $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens, at any prompt length. That is half of M3's base rate, and a quarter of M3's rate above 512K tokens. It is a MoE model with about 428B total and 23B active parameters, a 1M-token context, and text, image and video input. It always reasons. Weights are on Hugging Face under the MiniMax Community License, which is non-commercial by default and adds conditions for commercial use.
MiniMax has shipped the follow-up to M3 four and a half months after M3 appeared on OpenRouter. M3.1 Flash is cheaper, scores higher on independent tests, and comes with downloadable weights. MiniMax's launch post also confirmed what many developers had guessed: **Space Bunny**, the anonymous stealth model that topped OpenRouter's usage charts in late September, was an early version of M3.1 Flash.

The model is aimed at agentic coding, computer use and long-context tool loops. It is one of the cheapest agent-grade models with downloadable weights. GLM-5.3 Flash lists a slightly lower output price, but M3.1 Flash has the lowest Artificial Analysis cost per task ($0.22) among the open-weight models in its score band. Three details change how you integrate it. You cannot turn reasoning off. The license is not a permissive open-source license. And MiniMax's speed claims are higher than what independent measurements show.

This guide covers the specs and pricing (with worked cost math against M3), the architecture, how the always-on reasoning API behaves, vendor benchmarks next to independent scores, the open-weights license and serving requirements, the M Plan subscription that replaced Token Plan, and how to set up Claude Code and Cursor. Unless noted otherwise, all figures were checked against MiniMax's docs, the Hugging Face model card, OpenRouter's model API and Artificial Analysis on 26 October 2026.

## MiniMax M3.1 Flash specs at a glance

| Item | MiniMax M3.1 Flash | 
|---|---|
| General availability | 19 October 2026 | 
| MiniMax API model ID | `MiniMax-M3.1-Flash` (the earlier`MiniMax-M3.1-Flash-Preview` is retired, and requests using it are served by the GA model) | 
| OpenRouter ID | `minimax/minimax-m3.1-flash` | 
| Hugging Face repo | `MiniMaxAI/MiniMax-M3.1-Flash` | 
| Architecture | Mixture-of-experts, about 428B total and 23B active parameters per token | 
| Context window | 1,000,000 tokens per MiniMax's API docs (OpenRouter lists 1,048,576), with up to 524,288 output tokens on OpenRouter's top provider | 
| Input / output | Text, image and video in; text out | 
| Reasoning | Always on; `reasoning_effort` from`low` to`max` , default`max` | 
| Weights license | MiniMax Community License (non-commercial by default, with conditions for commercial use) | 
| API price (as of October 2026) | $0.15 input / $0.60 output / $0.015 cached input per 1M tokens, flat at any length | 

## Pricing: how much cheaper than M3?

MiniMax's pricing page lists M3.1 Flash at a single flat rate. M3 is still sold in two context bands and two service tiers. The M3 prices below already include MiniMax's "permanent 50% off".

| Model / band (Standard tier) | Input per 1M | Output per 1M | Cache read per 1M | 
|---|---|---|---|
| MiniMax-M3.1-Flash (any length) | $0.15 | $0.60 | $0.015 | 
| MiniMax-M3, up to 512K input tokens | $0.30 | $1.20 | $0.06 | 
| MiniMax-M3, above 512K input tokens | $0.60 | $2.40 | $0.12 | 

M3 also has a Priority tier at 1.5x the Standard price. M3.1 Flash does not accept the `service_tier` parameter, so it has no priority option.

Cached input matters most for agents. On M3.1 Flash a cache read costs one-tenth of the input price. On M3 it costs one-fifth. Long agent loops re-send the same system prompt, tool definitions and file contents on every turn, so they gain twice: the base rate is lower, and the cache discount is deeper.

### Worked example 1: a coding-agent session

Assume one session sends 2,000,000 input tokens, 80% of which are cache hits, and generates 200,000 output tokens, reasoning included. Every request stays under 512K tokens.

- **M3.1 Flash:** 0.4M uncached × $0.15 = $0.060; 1.6M cached × $0.015 = $0.024; 0.2M output × $0.60 = $0.120.**Total: $0.204.**
- **M3 (≤512K band):** 0.4M × $0.30 = $0.120; 1.6M × $0.06 = $0.096; 0.2M × $1.20 = $0.240.**Total: $0.456.**

M3.1 Flash costs about 45% of M3 here ($0.204 / $0.456 ≈ 0.447). MiniMax says agentic usage costs about 70% less than M3. That would need a larger change than list prices alone: fewer tokens per task as well as lower rates. Check it against your own token logs before you budget on it.

### Worked example 2: one long-context request

Take a single request with a 700,000-token prompt (uncached) and a 20,000-token answer.

- **M3.1 Flash:** 0.7M × $0.15 = $0.105; 0.02M × $0.60 = $0.012.**Total: $0.117.**
- **M3 (>512K band):** 0.7M × $0.60 = $0.420; 0.02M × $2.40 = $0.048.**Total: $0.468.**

Above 512K tokens, M3.1 Flash is exactly four times cheaper ($0.468 / $0.117 = 4.0).

### OpenRouter pricing and providers

OpenRouter lists the same $0.15 / $0.60 / $0.015 headline rates. It ran the model free for the first 72 hours after launch. As of 26 October, OpenRouter's endpoint list shows 11 endpoints from 10 providers, including MiniMax, Together, Nebius, Novita, Parasail and DigitalOcean. Only GMICloud undercuts the list price, serving an FP8 build at about $0.1425 / $0.57 (a 5% discount). Most of the rest, including InferenceNet's FP4 build, charge the $0.15 / $0.60 list price; SambaNova charges more, at $0.30 / $1.20. Quantized third-party endpoints may not match MiniMax's own results, so pin providers in production if consistency matters.

## Architecture: what changed from M3

According to the Hugging Face model card, M3.1 Flash is a natively multimodal MoE model with about 428B total parameters and 23B active per token. Three changes from M3 stand out:

- **MiniMax Sparse Attention (MSA) in every layer.** M3 used full attention in its first three layers. M3.1 Flash replaces those with MSA too, which helps keep long-context inference affordable at 1M tokens. MiniMax describes MSA in its paper arXiv 2606.13392.
- **Quantization-aware training** on the routed MoE experts and the attention paths. The released checkpoint is mostly 8-bit and 4-bit (MXFP8 and packed low-bit tensors), not BF16.
- **A DSpark draft head for speculative decoding.** It replaces the EAGLE-style multi-token prediction used before and ships in the checkpoint's`dspark/` folder.

One detail confuses people: Hugging Face's parameter counter shows about 220B, not 428B. The repo metadata shows roughly 206.6B of those "parameters" are stored as U8. If each byte packs two 4-bit weights, the count works out to about 413B plus roughly 14B of FP8, BF16 and FP32 tensors, close to the card's 428B. That is our reading of the metadata, not a MiniMax statement. The repo's safetensors index lists about **250 GB of weights** across 48 shards. The DSpark draft head adds about 2.3 GB, so plan for roughly 252 GB of downloads.

## The always-reasoning API

M3.1 Flash always reasons. That is the biggest behavioural difference from M3, which still lets you skip thinking. MiniMax's OpenAI-compatible API reference spells out the rules:

- `reasoning_effort` accepts`low` ,`medium` ,`high` ,`xhigh` and`max` .**If you omit it, the default is `max`.**`none` is rejected with a 400.
- `thinking: {"type": "disabled"}` also returns a 400 (error code 2013).`adaptive` is accepted.
- Reasoning comes back in a separate `reasoning_content` field.`reasoning_split=false` , which would inline the reasoning in`<think>` tags, is not supported.
- Thinking tokens count toward `max_tokens` /`max_completion_tokens` . If the limit is too small, you get`finish_reason: "length"` with empty`content` .
- `temperature` and`top_p` are ignored. The model always runs at 1.0 and 0.95.
- In multi-turn tool calling, append the *complete* assistant message to history, including`tool_calls` and`reasoning_content` , so the reasoning chain stays continuous.

In practice, the default `max` effort is the most expensive and slowest setting. For classification, extraction or simple edits, set `low` or `medium` explicitly and keep `max` for multi-step agent work.

```
import os
from openai import OpenAI
client = OpenAI(
    base_url="https://api.minimax.io/v1",
    api_key=os.environ["MINIMAX_API_KEY"],
)
messages = [
    {"role": "system", "content": "You are a senior Python reviewer."},
    {"role": "user", "content": "Find the bug: def avg(xs): return sum(xs) / len(xs) - 1"},
]
resp = client.chat.completions.create(
    model="MiniMax-M3.1-Flash",
    reasoning_effort="medium",      # omit and you get "max"
    max_completion_tokens=8192,     # thinking tokens count toward this
    messages=messages,
)
msg = resp.choices[0].message
print("Reasoning:", msg.reasoning_content)
print("Answer:", msg.content)
# For tool-calling loops, keep the full assistant turn, including
# tool_calls and reasoning_content, before appending tool results.
messages.append(msg.model_dump(exclude_none=True))
```
MiniMax also offers an Anthropic-compatible endpoint at `https://api.minimax.io/anthropic` and an OpenAI Responses-compatible endpoint. Both take the same model ID. Image and video go in as `image_url` and `video_url` content parts. URL or base64 videos can be up to 50 MB, and videos uploaded through the Files API can be up to 512 MB.

## Benchmarks: vendor card vs independent scores

The scores below are **vendor-reported by MiniMax** on the Hugging Face model card. Each benchmark used a different harness: Terminal Bench 4.0 ran in Codex 0.149.1, DeepSWE v1.1 in mini-SWE-agent 2.4.5 at 512K context, and several others in specific Claude Code builds. Treat them as directional.

| Benchmark (MiniMax-reported) | M3.1 Flash | DeepSeek V4.1 Flash-1017 | GLM-5.3 | GPT-5.6 Sol | Opus 5 | 
|---|---|---|---|---|---|
| DeepSWE v1.1 | 73.7 | 77.0 | 66.9 | 73.0 | 74.0 | 
| Terminal Bench 4.0 | 38.1 | 42.3 | 37.9 | 39.9 | 51.8 | 
| OSWorld 2.0 | 61.7 | — | — | 65.7 | 70.6 | 
| Automation-Bench v1.0.6 | 62.8 | 60.5 | 48.8 | 45.8 | 50.3 | 
| BankerToolBench | 76.2 | — | 74.5 | 74.6 | 76.2 | 
| NL2Repo-Bench | 61.2 | 68.2 | 58.0 | 56.8 | 75.3 | 
| CyberGym | 82.2 | — | 84.5 | 83.6 | — | 
| HLE (text-only, no tools) | 43.4 | 42.6 | 42.3 | 49.5 | 55.6 | 

A dash means MiniMax did not report a score. The card's GLM-5.3 column is the full GLM-5.3, not GLM-5.3 Flash. Even on MiniMax's own card, M3.1 Flash trails DeepSeek V4.1 Flash-1017 on DeepSWE, Terminal Bench and NL2Repo. It leads on Automation-Bench and ties Opus 5 for the top BankerToolBench score (76.2). Both are tool-heavy workflow tests.

### Independent: Artificial Analysis

On the Artificial Analysis leaderboard as of 26 October, MiniMax-M3.1-Flash scores **43** on the Intelligence Index (42.7 unrounded). MiniMax-M3 scores 29 (29.2). Cost per task fell from $0.51 to $0.22. Nearby models on the same leaderboard:

| Model (AA leaderboard, 26 Oct 2026) | Intelligence Index | Cost per task | Median output tokens/s (live, snapshot ~19:30 UTC) | OpenRouter list price (in / out per 1M) | 
|---|---|---|---|---|
| MiniMax-M3.1-Flash | 43 | $0.22 | 119 | $0.15 / $0.60 | 
| MiniMax-M3 | 29 | $0.51 | 107 | $0.30 / $1.20 | 
| DeepSeek V4.1 Flash 1017 (max) | 45 | $0.26 | 169 | $0.30 / $1.20 at peak; $0.15 / $0.60 off-peak | 
| GLM-5.3-Flash | 42 | $0.28 | 75 | $0.15 / $0.50 | 
| Claude Haiku 5.5 (max) | 43 | $0.21 | 234 | $0.10 / $0.50 | 

Speed figures are live medians and drift by a few tokens/s from hour to hour, so treat them as approximate. A gain of about 13.5 points over M3 (42.7 vs 29.2 unrounded) is a large jump for a point release. M3.1 Flash now sits in the same band as the other low-cost agent models. DeepSeek V4.1 Flash 1017 scores two points higher. On OpenRouter it costs twice M3.1 Flash's list price during weekday peak windows (01:00 to 04:00 and 06:00 to 10:00 UTC) and the same $0.15 / $0.60 the rest of the time. On AA cost per task the gap is about 18% ($0.26 vs $0.22).

### Speed: claimed vs measured

MiniMax's launch post says output can reach 200+ tokens per second in everyday use, with a median (P50) above 170 tokens/s. It credits more aggressive sparsity at inference time. Independent numbers are lower. Artificial Analysis measures a median of **about 116 to 119 tokens/s** (the live figure moved within that range across our checks on 26 October), with roughly 1.0 to 1.2 seconds to first token. During the Space Bunny run, OpenRouter's figures (as reported by Unite.AI) showed throughput rising from about 70 to about 100 tokens/s. Over the same period, time to first token dropped from 1.95 s to 1.22 s and the error rate halved from 1.4% to 0.7%. Plan capacity around 100 to 120 tokens/s on shared endpoints until you have measured your own provider.

## Where M3.1 Flash came from (the Space Bunny stealth run)

MiniMax's launch post (19 October) confirms that Space Bunny was an early version of M3.1 Flash. OpenRouter, as reported by Unite.AI, says Space Bunny was its number-one model for 10 straight days and processed **56.7 trillion tokens** on its platform, the most of any stealth model it has hosted. MiniMax's launch post says Space Bunny "processed more than 200 trillion tokens while operating anonymously", a figure that presumably includes traffic outside OpenRouter. The stealth endpoint went offline on 5 October and returned as M3.1 Flash on 19 October. For the stealth period itself, see our Space Bunny Alpha write-up.

The same post says a Pro model, "which pushes the boundaries of intelligence, will also arrive later." It has not given a release date or published specs.

## Open weights: what the MiniMax Community License allows

The `MiniMaxAI/MiniMax-M3.1-Flash` repository on Hugging Face was created on 17 October. The 19 October launch post said weights would come later; as of 26 October they are public. Engineering leaders should read the license before anything else, because it is not MIT or Apache. The MiniMax Community License grants rights for **non-commercial purposes**. Commercial use comes with conditions:

- You must prominently display "Built with MiniMax M3.1-Flash" on a related website, UI, blog post, about page or product documentation.
- If the product or service makes more than **$20 million a year** in revenue, you need prior written authorization from MiniMax. Below that, you send a one-time notice to MiniMax's API team.
- "Commercial use" is defined broadly. It covers paid third-party products, commercial use of APIs serving the model, and commercial deployment of fine-tuned or otherwise post-trained derivatives.
- An appendix of prohibited uses includes any military purpose.

For an internal tool or a startup product, the attribution and notice steps are small. For a vendor that resells inference or ships a fine-tuned derivative, have legal review the license before you build on it. If you are comparing open-weight options, check their licenses side by side. Our DeepSeek V4.1 Flash guide and GLM-5.3 Flash guide cover the two closest alternatives.

## Serving M3.1 Flash with vLLM or SGLang

"Flash" does not mean small. At about 250 GB of weights plus KV cache, this is a multi-GPU, data-center deployment. The official recipes target:

| Engine | Hardware in the official recipe | Notes | 
|---|---|---|
| vLLM 0.33.0+ | TP4 on GB200/GB300/B200/B300 or AMD MI355X; TP8 on H100/H200 | The Hopper config uses FP8 KV and Marlin experts, caps context at 262,144 tokens, disables video input and runs without DSpark | 
| SGLang (dev build) | 4x B300 (TP4, low latency) or 8x B200 (TP8, high throughput); AMD MI350X/MI355X at TP4 | Not yet in SGLang `main` ; use the`lmsysorg/sglang:dev-minimax-m31-flash` image. Blackwell needs CUDA toolkit 13.4+ | 

Both engines need MiniMax's M3 parsers for reasoning and tool calls: `minimax_m3` in vLLM and `minimax-m3` in SGLang. In vLLM, `--block-size 128` is mandatory for the MSA index cache. Here is the vLLM recipe command for a single 8x H100/H200 node:

```
vllm serve MiniMaxAI/MiniMax-M3.1-Flash \
  --tensor-parallel-size 8 \
  --reasoning-parser minimax_m3 \
  --block-size 128 \
  --kv-cache-dtype fp8 \
  --attention-config '{"indexer_kv_dtype": "fp8"}' \
  --compilation-config '{"cudagraph_mode": "FULL_AND_PIECEWISE"}' \
  --max-model-len 262144 \
  --max-num-seqs 80 \
  --limit-mm-per-prompt '{"video": 0}' \
  --gpu-memory-utilization 0.9 \
  --no-enable-log-requests \
  --disable-hybrid-kv-cache-manager \
  --moe-backend marlin \
  --enable-auto-tool-choice \
  --tool-call-parser minimax_m3
```
Set `reasoning_effort` through `chat_template_kwargs` when you self-host. Expect slow first launches on Blackwell while kernels compile. The model card says Transformers, KTransformers and Unsloth support is still in progress, so consumer-GPU and GGUF routes are not ready yet. If you have M3 running locally, our guide to running MiniMax M3 locally covers the general workflow, but M3.1 Flash needs the new parsers and builds listed above.

For most teams the math favours the API. At $0.15 / $0.60 per million tokens, an 8-GPU H100 node pays off only at sustained high volume, or when data residency or the license terms push you to self-host.

## M Plan: the subscription that replaced Token Plan

For individual developers using coding agents, MiniMax now sells the M Plan. Token Plan is closed to new purchases. Existing Token Plan subscribers can keep their plan, or upgrade with a prorated credit, but cannot switch back afterwards. MiniMax says M Plan keeps Token Plan's prices and text usage limits.

| Tier | Monthly price | Usage vs Go | Text model | Video (H3) | Price per Go-equivalent unit | 
|---|---|---|---|---|---|
| Go | $22 | 1x | M3.1 Flash | Not included | $22.00 | 
| Explore | $55 | 3x | M3.1 Flash | Included | $18.33 ($55 / 3) | 
| Build | $132 | 7.5x | M3.1 Flash | Included | $17.60 ($132 / 7.5) | 

Usage resets on two independent windows: a 5-hour window for text, image and audio models, and a weekly window for everything. Text requests need headroom in both. Rate limits are separate from usage limits, so heavy concurrency can be throttled before you run out of quota. The M Plan Subscription Key is not the same as a pay-as-you-go API key, and the two are not interchangeable. Use a pay-as-you-go key for production services. A 50%-off first-month promotion ran "through October 14" and applied only to monthly billing. Check current pricing before you subscribe.

## Setting up Claude Code and Cursor

### Claude Code

MiniMax's Claude Code guide points Claude Code at the Anthropic-compatible endpoint through `~/.claude/settings.json`. First unset any exported `ANTHROPIC_AUTH_TOKEN` and `ANTHROPIC_BASE_URL`, because shell variables override the settings file.

```
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.minimax.io/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "<MINIMAX_API_KEY>",
    "CLAUDE_CODE_AUTO_COMPACT_WINDOW": "524288",
    "CLAUDE_CODE_EFFORT_LEVEL": "max",
    "ANTHROPIC_MODEL": "MiniMax-M3.1-Flash[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "MiniMax-M3.1-Flash[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "MiniMax-M3.1-Flash[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "MiniMax-M3.1-Flash[1m]"
  }
}
```
MiniMax recommends auto-compacting at 512K tokens instead of letting sessions run to the full 1M. Once `CLAUDE_CODE_EFFORT_LEVEL` is set, the in-session `/effort` command has no effect. Remove the variable if you want to switch effort per task. Settings that disable thinking, such as `MAX_THINKING_TOKENS=0`, are ignored because the model always thinks. MiniMax's guide puts your M Plan Subscription Key in `ANTHROPIC_AUTH_TOKEN`. These are the same base URLs used for pay-as-you-go API calls, so a pay-as-you-go key should also work, billed per token instead of against your plan quota. Run `/status` and `/model` to confirm Claude Code is routing to MiniMax. MiniMax also offers a setup wizard (`npx -y mmx-cli@latest agent setup`) that writes this configuration for you.

### Cursor

In Cursor's **Models** settings, enable "Override OpenAI Base URL" and set it to `https://api.minimax.io/v1`. Paste your key into the OpenAI API Key field (MiniMax's guide uses the M Plan Subscription Key here), then add a custom model named exactly `MiniMax-M3.1-Flash`. Custom models require Cursor Pro or above. The override applies to *every* OpenAI-style key in Cursor, so built-in Claude and GPT models can break while it is on. Tab autocomplete still uses Cursor's own model. See MiniMax's Cursor guide for the HTTP/1.0 fallback if responses come back empty.

## Who should use M3.1 Flash?

- **Good fit:** high-volume agent loops with heavy prompt caching, long-context work above 512K tokens (where it is 4x cheaper than M3), multimodal pipelines that need video input, and teams that want an open-weight fallback for their hosted API.
- **Weigh carefully:** latency-sensitive features. Measured throughput is about 100 to 120 tokens/s, and reasoning cannot be disabled, so even simple calls produce thinking tokens.
- **Check first:** commercial self-hosting, inference resale and fine-tuned derivatives, all of which fall under the license conditions above.

If you are moving from M3, the migration is mostly a model-string change. Two things need attention. Set `reasoning_effort` explicitly, since the default is `max`. And remove any `thinking: {"type": "disabled"}` or `service_tier` parameters, because the first returns a 400 and the second is not supported. Our MiniMax M3 developer guide and M3 launch overview cover the baseline you are migrating from. For a wider price comparison, see cheapest fast LLM APIs compared.

## FAQ

### What is the MiniMax M3.1 Flash API model ID?

On MiniMax's API it is `MiniMax-M3.1-Flash`, on both the OpenAI-compatible and Anthropic-compatible endpoints. On OpenRouter it is `minimax/minimax-m3.1-flash`. The old `MiniMax-M3.1-Flash-Preview` name is retired, and requests using it are routed to the GA model.

### How much does MiniMax M3.1 Flash cost?

As of October 2026: $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens, flat at any context length. M3 costs $0.30 / $1.20 up to 512K tokens and $0.60 / $2.40 above that.

### Can I turn off reasoning in M3.1 Flash?

No. Sending `thinking: {"type": "disabled"}` or `reasoning_effort: "none"` returns a 400 error. The lowest setting is `reasoning_effort: "low"`. The default is `max`.

### Is MiniMax M3.1 Flash open source?

The weights are downloadable, but the MiniMax Community License is not an OSI open-source license. It allows non-commercial use freely. Commercial use requires a "Built with MiniMax M3.1-Flash" attribution and a one-time notice to MiniMax. If the product or service using the model generates more than $20M a year, you need prior written authorization from MiniMax instead of the notice.

### Is M3.1 Flash the same as Space Bunny?

Yes. MiniMax confirmed that the Space Bunny stealth model on OpenRouter was an early version of M3.1 Flash. OpenRouter says it processed 56.7 trillion tokens on its platform before going offline on 5 October.

### When is M3.1 Pro coming?

MiniMax has said a Pro model will follow Flash, but it has not given a date or specs.

## Sources

If your team is building agents on MiniMax or other open-weight models and needs engineers who have handled evaluation, serving and licensing reviews before, Codersera can help you hire vetted remote AI developers.
