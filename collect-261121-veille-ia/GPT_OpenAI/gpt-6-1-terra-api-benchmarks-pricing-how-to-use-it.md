---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-api-benchmarks-pricing-how-to-use-it
title: "GPT-6.1 Terra: API, Benchmarks, Pricing & How to Use It"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-26"]
keywords: ["terra", "gpt-6", "pricing", "tier", "benchmarks", "models", "openai", "frontier"]
source: https://www.truefoundry.com/blog/gpt-6-1-terra
source_anchor: ""
source_lines: [1, 105]
section: "GPT_OpenAI"
---
# GPT-6.1 Terra: API, Benchmarks, Pricing & How to Use It

### Built for Speed: ~10ms Latency, Even Under Load

Blazingly fast way to build, track and deploy your models!

- Handles 350+ RPS on just 1 vCPU — no tuning needed
- Production-ready with full enterprise support

## What is GPT-6.1 Terra?

OpenAI now ships named tiers within each model generation — roughly **Astra** (flagship), **Sol**, **Terra** and **Luna** — where the number (6.1) marks the generation and the name marks the capability-and-cost tier. **Terra** is the balanced, production tier: not the absolute frontier on raw reasoning, but tuned to be fast and cheap enough to run agentic and computer-use workloads at volume.

The defining capability this release is **computer use**. Terra pairs visual understanding with the ability to operate browser and desktop applications, surfaced as a hosted tool in the Responses API. In plain terms: it can look at a screen and click, type and navigate through it — the core loop behind practical computer-using agents — at a price that makes those loops economical.

## Key specifications

## Pricing

Terra’s pricing is the story. At $1/$5 per million tokens it undercuts frontier models several times over, and the batch/flex discount halves that again for asynchronous work.

Two cost traps worth flagging: **fast mode doubles the rate**, and any request whose input exceeds **272K tokens** is billed at 2x input/cache and 1.5x output for the *entire* request. Long-context agent loops can quietly cross that line, so measure actual token shapes rather than assuming the headline $1/$5.

## Benchmarks

OpenAI’s launch-day figures emphasize security, multimodal troubleshooting and health reasoning:

- **ExploitBench:** 97.10%
- **Multimodal Troubleshooting (Virology):** 52.54%
- **HealthBench Professional:** 62.7 (up from 57.7 for GPT-5.6 Terra)
- **Standard HealthBench:** 55.1 (down slightly from 57.0 for GPT-5.6 Terra)
- **Sandbox Bench:** 5 of 22 targets

Read these as launch-day vendor numbers, not a full production verdict. Notably, Terra is not uniformly ahead of its predecessor on every benchmark (standard HealthBench dipped), which is a good reminder that tier models trade raw capability for speed and cost. OpenAI also claims Terra is **more than 3x faster** than GPT-6 Astra and GPT-6.1 Sol — a meaningful latency edge for interactive agents, but a vendor claim from internal testing.

## How GPT-6.1 Terra compares

The useful comparison is on price-for-capability against the models you would otherwise route to. All figures are per million tokens and reflect published launch pricing (verify before relying on them).

The takeaway: Terra sits in the same low-cost band as Gemini 3.6 Flash and well below Claude Opus 4.8 and Fable 5.1, while bringing first-class computer use. It is not trying to beat Fable 5.1 or GPT-6.1 Sol on the hardest reasoning — it is trying to make the *routine* 80% of agent work cheap. That is precisely the split a gateway is built to exploit: Terra for volume, a frontier model for the hard tail.

## Limitations

- **Tier, not frontier.** Terra trades peak reasoning for speed and cost. For the hardest long-horizon tasks, Fable 5.1 or a flagship tier will still win.
- **Long-context cost cliff.** Requests over 272K tokens are billed at 2x input/1.5x output for the whole request — easy to trigger accidentally in long agent loops.
- **No fine-tuning.** Terra does not support fine-tuning at launch.
- **Access gaps.** Not available in regular ChatGPT Chat, Free or Go at launch, and not available for UAE inference residency.
- **Text-only output.** Inputs are text and image; output is text (no audio or video).

## How to access and use GPT-6.1 Terra

There are two practical paths to production.

### 1. Call the OpenAI API directly

Terra is available on the OpenAI API at the Build tier and above, with computer use exposed as a hosted tool in the Responses API. This is the fastest way to prototype a single app against a single model.

### 2. Route it through the TrueFoundry AI Gateway (recommended for production)

The moment you run Terra alongside other models — Claude for hard reasoning, Gemini for cheap multimodal, Terra for computer use — you need routing, fallbacks, budgets and one cost ledger. That is what the TrueFoundry AI Gateway provides: a single OpenAI-compatible endpoint in front of every provider.

1. Point your client at the TrueFoundry gateway endpoint and select gpt-6.1-terra as the model.
2. Add routing rules — send computer-use and high-volume calls to Terra, escalate the hard tail to a frontier model, with automatic fallback if a provider errors or rate-limits.

You get per-key, per-team and per-agent budgets, a cross-provider cost and token ledger, and full audit — without rewriting application code when prices or models change. The gateway adds only a few milliseconds of overhead and is OpenTelemetry-compliant.

Put GPT-6.1 Terra behind one governed, multi-provider endpoint with budgets and fallbacks. Talk to the TrueFoundry team →

### When to use GPT-6.1 Terra

- **Reach for Terra when:** the workload is computer use or agentic automation; volume is high and cost matters; you need a large context at a low per-token rate; latency matters for an interactive agent.
- **Escalate to a frontier model when:** the task is genuinely hard long-horizon reasoning or complex coding where a wrong answer costs more than the tokens — route those to Claude Fable 5.1, Opus 4.8 or GPT-6.1 Sol.
- **Consider Gemini 3.6 Flash when:** you want the cheapest fast multimodal option and don’t need Terra’s computer-use tool.

TrueFoundry AI Gateway delivers ~3–4 ms latency, handles 350+ RPS on 1 vCPU, scales horizontally with ease, and is production-ready, while LiteLLM suffers from high latency, struggles beyond moderate RPS, lacks built-in scaling, and is best for light or prototype workloads.

## Recent Blogs

## Frequently asked questions

### What is GPT-6.1 Terra?

GPT-6.1 Terra is OpenAI’s cost-efficient tier model released October 26, 2026, optimized for computer use and agentic work. It offers a 1.05M-token context and $1/$5 per-million-token pricing, and pairs visual understanding with the ability to operate browser and desktop apps.


### How much does GPT-6.1 Terra cost?

$1.00 per million input tokens and $5.00 per million output tokens, with $0.05 cached input and a 50% discount on batch and flex processing. Fast mode costs 2x, and requests over 272K tokens are billed at 2x input / 1.5x output for the whole request.


### What is GPT-6.1 Terra’s context window?

1,050,000 tokens, with a maximum output of 128,000 tokens. Inputs can be text and image; output is text only.

### How does GPT-6.1 Terra compare to Claude Opus 4.8 and Fable 5.1?

Terra is far cheaper ($1/$5 vs $5/$25 for Opus 4.8 and $10/$50 for Fable 5.1) and brings computer use, but it is a tier model rather than a frontier reasoner. A common production pattern is to route routine and computer-use work to Terra and the hard reasoning tail to Opus 4.8 or Fable 5.1 through a gateway.

### Where can I use GPT-6.1 Terra?

On the OpenAI API (Build tier and above), in ChatGPT Work and Codex, and in GitHub Copilot. It is not in regular ChatGPT Chat, Free or Go at launch, and is not available for UAE inference residency. For production across multiple models, teams typically route it through an AI gateway.

### Is GPT-6.1 Terra good for computer-use agents?

That is its core use case. Computer use is exposed as a hosted tool in the Responses API, and Terra’s low price makes the repeated screen-read-and-act loop of a computer-using agent economical at scale.
