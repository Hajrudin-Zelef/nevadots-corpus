---
id: collect-261121-veille-ia/claude-anthropic/is-claude-fable-5-2-worth-it-benchmarks-pricing-and-how-to-use-it
title: "Is Claude Fable 5.2 Worth It? Benchmarks, Pricing, and How to Use It"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "high", "model", "effort", "xhigh", "tokens", "coding"]
source: https://trtc.io/blog/details/anthropic-fable-5-2
source_anchor: ""
source_lines: [1, 133]
section: "Claude_Anthropic"
---
Anthropic released **Claude Fable 5.2 on November 9, 2026**. In **Anthropicâs launch announcement**, the company lists API prices of **$8 per million input tokens and $40 per million output tokens**, both 20% lower than Fable 5.1. The new model also posted a sizable improvement on repository-level coding and scientific research tests.

For routine coding, Opus 5.5 costs half as much at the same token volume. Fable 5.2 is worth testing when a difficult migration, bug, or research task keeps taking several attempts. Start at High effort and compare how much work each model gets right on the first pass.

## Claude Fable 5.2 Specs

The **Claude Fable 5.2 model reference** lists a one-million-token context window and an output limit of 128,000 tokens. Adaptive thinking stays on, and you can control its depth through the effort setting.

| Specification | Claude Fable 5.2 | 
|---|---|
| Claude API model ID | `claude-fable-5-2` | 
| Context window | 1 million tokens | 
| Maximum output | 128,000 tokens | 
| Input and output | Text and images in, text out | 
| Effort levels | Low, Medium, High, Xhigh, Max | 
| Default effort | High on the API, Medium in Claude apps and Claude Code | 
| Reliable knowledge cutoff | June 2026 | 
| Access | Eligible Claude plans, Claude API, and supported AWS, Google Cloud, and Microsoft Foundry deployments | 

The **Fable 5.2 and Mythos 5.2 system card** describes how the two versions share an underlying model. Fable is generally available with additional cybersecurity and biology safeguards, while Mythos access is limited to verified organizations. For production use, **Anthropicâs deployment requirements** specify 30-day data retention unless Anthropic expressly authorizes an exception.

## Fable 5.2 vs. Fable 5.1, Opus 5.5, GPT-6 Astra, and Gemini 4 Carbon

The **benchmark results Anthropic published at launch** use the highest available effort for each model unless the table specifies a setting.

| Benchmark | Fable 5.2 | Fable 5.1 | Opus 5.5 | GPT-6 Astra | Gemini 4 Carbon | 
|---|---|---|---|---|---|
| FrontierCode 2.0 Â· coding | **49.1%** (Xhigh) | 32.4% (Xhigh) | 45.5% | 42.8% | 41.1% (Medium) | 
| FrontierSWE v2 Â· coding | **69.9%** | 56.3% | 62.3% | 65.5% | 61.2% | 
| Terminal-Bench-Science 0.1 Â· research | **74.6%** | 52.6% | 58.7% | 68.1% | 64.8% | 
| AutomationBench Â· business workflows | 51.4% | 31.4% | 42.5% | 41.4% | **53.9%** | 

At the same Xhigh setting, **Fable 5.2 gained 16.7 percentage points over Fable 5.1** on FrontierCode 2.0. At High, it scored 45.3%, close to Opus 5.5 at Max at 45.5%. In that particular test setup, Anthropic reported roughly half the cost per task for Fable at High. Gemini 4 Carbon led AutomationBench, so a team choosing a model for business automation has a different comparison to make than a team fixing bugs across a large codebase.

### What higher effort costs in independent testing

The **Artificial Analysis Intelligence Index** compares models across several evaluations and reports a weighted cost per benchmark task. Its Fable measurements use default fallback.

| Model and effort | Intelligence Index | Cost per benchmark task | 
|---|---|---|
| Fable 5.2 Â· Max | 65 | $26.54 | 
| Fable 5.2 Â· Xhigh | 62 | $14.83 | 
| Fable 5.2 Â· High | 62 | $9.08 | 
| Gemini 4 Carbon Â· High | 60 | $7.76 | 
| Opus 5.5 Â· Max | 59 | $10.04 | 
| GPT-6 Astra Â· Max | 57 | $6.24 | 
| Fable 5.2 Â· Medium | 56 | $3.80 | 

Fable 5.2 High and Xhigh both scored **62 after rounding**, while High cost **about 39% less per benchmark task**. If youâre evaluating Fable for a coding agent, run High first and retest failures at Xhigh. That gives you a way to measure which tasks benefit from the extra reasoning budget.

## Watch the Official Claude Fable 5.2 Demo

In **Claudeâs official robotics demo**, Fable 5.2 controls physical robot arms to solve a puzzle cube, open a bottle, and draw a robot. The setup gives the model camera feedback and a way to control the hardware.

## Claude Fable 5.2 API Pricing Compared

The example assumes **20,000 uncached input tokens and 4,000 billed output tokens** per request. The rates come from **Anthropicâs Claude pricing table** for Fable and Opus, **OpenAIâs GPT-6 Astra model page** for Astra, and **Googleâs Gemini 4 Carbon announcement** for Carbonâs introductory offer to eligible partners.

| Model | Input / 1M tokens | Output / 1M tokens | Example request | 
|---|---|---|---|
| **Claude Fable 5.2** | **$8** | **$40** | **$0.32** | 
| Claude Fable 5.1 | $10 | $50 | $0.40 | 
| Claude Opus 5.5 | $4 | $20 | $0.16 | 
| GPT-6 Astra Â· standard | $10 | $50 | $0.40 | 
| Gemini 4 Carbon Â· introductory, limited access | $2 | $10 | $0.08 | 

At this volume, 1,000 requests cost **$320 on Fable 5.2** and **$160 on Opus 5.5**, before caching, tool charges, or taxes. Agent runs can consume different amounts of reasoning and tool context even when they start with the same prompt, so log actual token use alongside the number of tasks completed successfully.

Under **Fable 5.2âs batch, cache, and Fast mode pricing**, batch processing costs $4 per million input tokens and $20 per million output tokens. Cache hits cost $0.20 per million tokens. Fast mode costs $16 input and $80 output per million tokens and can generate tokens up to 2.5 times as fast, according to Anthropic.

## How to Use Claude Fable 5.2

### Try it in Claude

Open **Claude on the web** with an eligible paid account, start a new chat, and choose **Fable 5.2** from the model picker. Try a task you can check, such as reconciling conflicting totals across two reports, then compare the answer with Opus 5.5 against the original documents.

Under the **Fable 5.2 launch offer**, eligible Pro and standard Team seats can use Fable for up to 50% of their weekly limit through November 16, 2026, at 11:59 p.m. Pacific Time. After the offer, those plans generally use paid usage credits for Fable.

### Use it in Claude Code to build a Tencent RTC video room

According to **Claudeâs Fable access guide**, Claude Code requires version 2.1.321 or later for Fable 5.2. From a React project, install **Tencent RTC Agent Skills** to give Claude Code product-specific guidance for building a two-person video room.

```
claude --version
npx -y @tencent-rtc/trtc-agent-skills@latest add
claude --model claude-fable-5-2
```
Ask Fable to inspect the app, integrate the official Tencent RTC meeting UI, and support a host and guest with distinct user IDs. The **Tencent RTC React meeting walkthrough** shows how to prepare test credentials and verify that two users can join the same room. For production, generate `UserSig` on your server and keep the signing secret there.

Test from two browser profiles. Confirm that both users can see and hear each other, then deny microphone permission in one profile and check how the app handles it. If youâre comparing Fable 5.2 with Opus 5.5, use the same starting code and requirements. Compare which implementation passes those checks and how much manual work it takes to finish.

### Call Fable 5.2 through the API

Create an API key in the **Claude Console**, set `ANTHROPIC_API_KEY` in your environment, and install the current Python SDK. The **Claude API effort guide** shows how to set the reasoning level per request.

`python -m pip install --upgrade anthropic````
from anthropic import Anthropic
client = Anthropic()
response = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=4096,
    output_config={"effort": "medium"},
    messages=[
        {
            "role": "user",
            "content": (
                "Design a test plan for an API that retries failed payments. "
                "Cover duplicate requests, timeouts, and recovery after a crash. "
                "Give each test an expected result."
            ),
        }
    ],
)
for block in response.content:
    if block.type == "text":
        print(block.text)
print(response.usage)
```
If youâre upgrading from Fable 5.1, the **Fable 5.2 migration guide** explains why editing earlier messages, the system prompt, or tools can invalidate preserved thinking blocks. Replay a multi-turn tool workflow before switching production traffic.

## Frequently Asked Questions

**Is Claude Fable 5.2 free?**

Itâs available on paid Claude plans, subject to plan limits or usage credits. The Free plan does not include Fable. Claude API requests are billed separately.

**Is Fable 5.2 faster than Fable 5.1?**

Anthropic reports up to 30% faster output generation. The time to finish a task still depends on reasoning effort, the number of tokens generated, and time spent waiting for tools.

**Should I switch from Opus 5.5 to Fable 5.2?**

Use Opus 5.5 as your baseline. Test Fable 5.2 High on the jobs where Opus regularly needs another attempt, then compare the total time and cost to get a result you can use.
