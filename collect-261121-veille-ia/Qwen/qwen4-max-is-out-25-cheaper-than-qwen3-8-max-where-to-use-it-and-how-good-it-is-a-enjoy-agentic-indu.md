---
id: collect-261121-veille-ia/qwen/qwen4-max-is-out-25-cheaper-than-qwen3-8-max-where-to-use-it-and-how-good-it-is-a-enjoy-agentic-indu
title: "Qwen4-Max is out, 25% cheaper than Qwen3.8-Max. Where to use it, and how good it is â Enjoy Agentic Industries"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen4-max", "tokens", "model", "token", "alibaba", "qwen3", "opencode", "november"]
source: https://enjoy.dev/blog/qwen4-max-preview/
source_anchor: ""
source_lines: [1, 168]
section: "Qwen"
---
- To
- Everyone curious about Qwen4, or choosing a model for Claude Code, Codex or OpenCode
- From
- Mo, Chief Executive
- Date
- November 12, 2026, 3:17 AM CST
- Re
- Qwen4-Max

# Qwen4-Max is out, 25% cheaper than Qwen3.8-Max. Where to use it, and how good it is

On November 16, 2026, China time, Alibaba released Qwen4-Max, the first full Qwen4 model, five days after its preview. QwenCloudâs API charges $1.50 per million input tokens and $4.50 output, a quarter less than Qwen3.8-Max, and itâs on Token Plan, OpenRouter and OpenCode Go. Its weights arenât out, and Artificial Analysis scores it 43, under Kimi K3.1 Proâs 46 and Claude Opus 5.5âs 59.

## What Alibaba released

On November 16, 2026, China time, the evening of November 15 in the US, Alibaba released Qwen4-Max, five days after it put Qwen4-Max-Preview on its Token Plan subscription. QwenCloudâs model changelog dates `qwen4-max` November 16 and calls it âa flagship multimodal model for text, image and video understanding, reasoning, coding, and agent tasks.â Reuters, in a report carried by Yahoo, led with its safety claims.

Alibabaâs launch post, âQwen4-Max: Your New Teammate Has Arrived,â fills in what the preview left out:

- **Size:** â2.4T backbone parameters, 71B activated per token, and an additional 328B N-gram Embedding parameters,â on a new architecture for the Qwen4 series. Alibaba says its base model activates â25% fewer parametersâ than Qwen3.8-Maxâs.
- **Context:** 1 million tokens. In thinking mode, thinking and the answer share up to 262,144 tokens; without thinking, the answer gets up to 131,072 (docs).
- **Input:** text, images and video.
- **Thinking:** on by default, now at`high` , with`low` and`xhigh` the other choices. The preview defaulted to`xhigh` .
- **Tools:** function calling, structured output, web search, MCP and a code interpreter.
- **APIs:** OpenAIâs Chat Completions and Responses, Anthropicâs Messages and Alibabaâs DashScope, which is how Claude Code, Codex and OpenCode connect to it.

The model ID is `qwen4-max`. The preview, `qwen4-max-preview`, stays on Token Plan as what Alibaba calls âthe separateâ model, with its own end date, below.

## Where you can use it, and what it costs

| Route | Price, checked November 16 | What you get | 
|---|---|---|
| QwenCloud API, international (Singapore) | $1.50 in, $4.50 out per 1M tokens | 1 million free tokens; cached input $0.075 | 
| QwenCloud API, Global and Beijing | $1.413 in, $4.24 out per 1M tokens | Global serves from Virginia, Frankfurt, Hong Kong and Tokyo | 
| OpenRouter | $1.50 in, $4.50 out per 1M tokens | `qwen/qwen4-max` , from one OpenRouter key | 
| OpenCode Go | $10 or $40 a month | Qwen4 Max at API prices, from a monthly allowance | 
| Token Plan Individual | $6 to $43 a month | Credits for Qwen4-Max and the planâs other models | 
| Token Plan Team Edition | $20 to $180 a seat a month | The same for teams | 
| Qoder | 0.4Ã Qoderâs credits | Alibabaâs own coding app | 
| QwenStudio, QwenWork | Not published | Alibaba names both; no plans or limits given | 

The pay-as-you-go API is new: during the preview, Alibaba said âPay-as-you-go access is not supported.â Its price list puts Qwen3.8-Max at $2 in and $6 out per million tokens in the same region, so Qwen4-Max costs a quarter less per token. Alibaba says it uses fewer tokens too, and that on Work360, its own benchmark of professional tasks, âthe average cost per task drops from $4.43 to $1.67.â The free tokens are for pay-as-you-go calls in Singapore, âseparate from Token Plan credits,â with eligibility on QwenCloudâs free-quota page.

OpenRouter listed the model the same evening at QwenCloudâs international prices. OpenCode Go, the subscription from the makers of OpenCode, is $10 a month, or $40 for Go Plus. It bills Qwen4 Max at $1.50 and $4.50 against a $15 monthly allowance for the model on Go, with up to 20% of it usable in a five-hour window and 50% in a week, and estimates thatâs about 2,310 requests a month.

Token Plan is Alibabaâs subscription for coding tools, billed in credits. Individual is $6 a month for 20,000 credits (Lite), $10 for 45,000, $18 for 80,000 or $43 for 200,000. Lite and Essential are $3 and $5 for accounts that have never bought an Individual plan, until 23:59:59 on November 30, China time. Alibaba doesnât publish how many credits a Qwen4-Max call uses: âThe Credits consumed per request are dynamically determined by model type, token usage, thinking mode, and tool calls.â So the APIâs per-token price is the only firm number.

## How good it is

The first independent results are in. Artificial Analysis scores Qwen4 Max 43 on its Intelligence Index, up from the 34 Qwen3.8 Max (0902) scored when we read its leaderboard on November 12. Here it is beside the models people compare it with, from the leaderboard on November 16:

| Model | Intelligence Index | Cost per index task | 
|---|---|---|
| Claude Fable 5.2 (max) | 65 | $26.54 | 
| Claude Opus 5.5 (max) | 59 | $10.04 | 
| GPT-6.1 Sol (max) | 55 | $1.27 | 
| Gemini 4 Argon (high) | 50 | $4.97 | 
| Kimi K3.1 Pro | 46 | $3.68 | 
| Qwen4 Max | 43 | $3.27 | 
| GLM-5.3 (max) | 32 | $5.01 | 

On Arenaâs WebDev leaderboard, where people vote between two modelsâ websites, Qwen4-Max (xHigh) is sixth at 1,750, marked preliminary, behind Claude Fable 5.2, Opus 5.5, Sonnet 5.5, Gemini 4 Carbon and GPT-6 Astra.

Alibabaâs own chart compares it with five models. The coding results:

| Benchmark | Qwen4-Max | Qwen3.8-Max | Opus 5.5 | GPT-6.1 Sol | GPT-6 Astra | Kimi K3.1 | 
|---|---|---|---|---|---|---|
| DeepSWE v1.1 | 75.7 | 69.3 | 74.2 | 75.2 | 74.1 | 75.5 | 
| Terminal-Bench 4.0 | 48.5 | 38.9 | 59.6 | 56.1 | 59.1 | 67.2 | 
| ProgramBench | 31.5 | 28.0 | 65.0 | 52.5 | 55.0 | 25.0 | 
| CodeArena (Elo) | 1750 | 1676 | 1816 | 1742 | 1763 | 1686 | 

These are the publisherâs own numbers, and they donât all favor it: Qwen4-Max leads on DeepSWE, by 0.2 points over Kimi K3.1, and trails Opus 5.5 badly on Terminal-Bench 4.0, agentic coding in a terminal, and ProgramBench, which has a model rebuild software from how it behaves. On Work360, Alibaba reports 52.69, up from 47.74 for Qwen3.8-Max.

On safety, Alibaba says it added âcode security checks on top of functionality checksâ in training, raising its pass rate for code thatâs both working and secure from 22.0% to 31.8% on one benchmark, and that âno capabilities posing large-scale cybersecurity risks have been observed.â Both are its own reviews.

## What happens to Qwen4-Max-Preview

The preview stays on Token Plan until it ends. Alibabaâs preview page now says the period âruns from 10:00 on November 11, 2026 to 24:00 on November 25, 2026 (UTC+8); model capabilities continue to be iterated and upgraded during the preview, and the model will be retired after the preview ends.â Thatâs 10 a.m. Central on November 25.

Until then, its credits cost âas low as 20% of the standard rate (5x usage),â and 8% between 22:00 and 08:00 China time, which is 9 a.m. to 7 p.m. Eastern. Alibaba lists no such discount for `qwen4-max` on Token Plan, though it hasnât published either modelâs standard rate.

It also hasnât said how the preview differs from `qwen4-max`, or what the previewâs ID does after November 25. When it retired `qwen3.8-max-preview`, Alibaba kept the old ID working: ârequests are automatically routed to `qwen3.8-max`.â It has made no such promise this time, so change your settings before the 25th.

## How to set it up

Choose how youâll pay first, because the key decides the address. A Token Plan key works only with `token-plan.maas.qwencloudapi.com`; a pay-as-you-go key starts with `sk-` and uses your regionâs address, such as `ap-southeast-1.maas.qwencloudapi.com` for Singapore. Mix them up and you get `401 invalid_api_key`.

**Claude Code.** Alibabaâs guide says: âTo use qwen4-max, set `ANTHROPIC_DEFAULT_OPUS_MODEL` to `qwen4-max` and keep `CLAUDE_CODE_MAX_CONTEXT_TOKENS` at `983616`.â With a Token Plan key, in a terminal:

```
export ANTHROPIC_AUTH_TOKEN="YOUR_API_KEY"
export ANTHROPIC_BASE_URL="https://token-plan.maas.qwencloudapi.com/apps/anthropic"
export ANTHROPIC_MODEL="auto"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="qwen3.6-flash"
export ANTHROPIC_DEFAULT_SONNET_MODEL="qwen3.8-flash"
export ANTHROPIC_DEFAULT_OPUS_MODEL="qwen4-max"
export CLAUDE_CODE_SUBAGENT_MODEL="auto"
export CLAUDE_CODE_MAX_CONTEXT_TOKENS="983616"
```
With a pay-as-you-go key, the guide uses `https://ap-southeast-1.maas.qwencloudapi.com/apps/anthropic` as the address instead. Either way, Claude Code started from that terminal sends everything to Alibaba, and your Claude plan isnât used. The main model above is `auto`, and Claude Codeâs model docs say these variables only set what its `opus`, `sonnet` and `haiku` aliases mean, so set `ANTHROPIC_MODEL` to `qwen4-max`, or pick Opus with `/model`, to be sure youâre testing the new model. If long answers get cut off, Alibaba suggests `CLAUDE_CODE_MAX_OUTPUT_TOKENS=64000`.

**Codex.** For Codex 0.94.0 and later, Alibabaâs guide adds a provider to `~/.codex/config.toml` and reads your key from `OPENAI_API_KEY`. For Token Plan:

```
model_provider = "Token_Plan"
model = "auto"
[model_providers.Token_Plan]
name = "Token_Plan"
base_url = "https://token-plan.maas.qwencloudapi.com/compatible-mode/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```
For pay-as-you-go, its provider is `QwenCloud`, with `base_url = "https://ap-southeast-1.maas.qwencloudapi.com/compatible-mode/v1"`. Then, in Alibabaâs words, âUse `codex -m qwen4-max` to select the model.â

**OpenCode.** Run `opencode`, type `/connect`, and choose âAlibaba Token Plan (China)â for a Token Plan key, âAlibaba (China)â for a pay-as-you-go key, or OpenCode Go for a Go subscription. Then `/models` lists Qwen4 Max; if it doesnât, update OpenCode. Alibabaâs guide notes that OpenCode caps a single answer at 32,000 tokens unless you raise it with `OPENCODE_EXPERIMENTAL_OUTPUT_TOKEN_MAX`.

One setting catches tools with a small output cap: `max_completion_tokens` âlimits thinking and the final answer together,â so a tight cap can come back empty because the thinking used all of it. Raise the cap, or turn the effort down to `low`.

## The rules that come with Token Plan

Token Plan is for working with an agent yourself, not for anything that runs on its own. Its terms say: âUse is limited to interactive use within programming and agent tools (such as Claude Code, Cursor, Qwen Code, Qoder, OpenClaw, etc.). It must not be used for automation scripts, custom application backends, or any non-interactive batch call scenarios.â Its FAQ adds âscheduled background tasks,â and calls outside that scope âmay result in subscription suspension or API Key banning.â You can set one key up âon multiple devices you own,â but sharing it âmay be considered a violation.â

Individual âdoes not currently support unsubscribingâ; you can only stop it renewing. Its only region is Singapore, but âthe model inference will be performed globally,â and âthe prompts and model outputs will involve cross-border data transfer.â On training, the Team Edition âdoes not use your conversation data for model training,â while the FAQ says Individual data âis governed by the service agreement.â

## Is Qwen4-Max open source?

No. Alibaba hasnât released its weights, and nothing on its pages says it will. Its launch post ends: âAnd Max is only the first look at Qwen4. Smaller models in the family are coming to open source, built on the same lineage and trained with the same generation of techniques.â It names no models, sizes, licenses or dates.

Running it yourself wasnât in the cards anyway, at 2.4 trillion parameters. Itâs also above the 2-trillion-parameter line in Chinaâs draft export rules, under which a new open release of a model that size would need an export license, one the draft calls automatic. Alibaba hasnât connected the two.

## Qwen4-Max in Enjoy

Enjoy runs OpenCode, and its **Model** column lists the models OpenCode offers from your providers (how to set it up). Add Alibaba, OpenCode Go or OpenRouter to OpenCode with `opencode auth login`, reopen the agent settings, and choose Qwen4 Max there. Your Claude Code conversations stay on your Claude plan, and you can hand a conversation from Claude Code to OpenCode and back, so the same task gets a second answer from Qwen4-Max without starting over.

Two limits. Donât put a Token Plan key in a scheduled workflow, since its terms forbid scheduled background tasks; a pay-as-you-go key bills each call instead, and a local model costs nothing per call. Enjoy runs scheduled workflows while your computer is awake and Enjoy is open. And Enjoy doesnât add Alibabaâs models to any other labâs account: you still pay Alibaba or the service you reach it through (Enjoyâs pricing).

## What to do now

1. **Try it on the free tokens.** Pay-as-you-go calls in Singapore come with 1 million free tokens for Qwen4-Max, if your account qualifies under QwenCloudâs free-quota rules: enough for a real test without a subscription.
2. **Test it on a task you know.** Keep your usual setup in another terminal, give both the same job, and compare the diffs, not the prose.
3. **If youâre on Token Plan, move to `qwen4-max` before November 25.** Use the discounted preview until then if you like, but donât leave`qwen4-max-preview` in a config: it retires at 10 a.m. Central that day.
4. **Keep another model for long terminal work.** By Alibabaâs own numbers, Opus 5.5, GPT-6 Astra and Kimi K3.1 all beat Qwen4-Max on Terminal-Bench 4.0 by 10 points or more.

## What Alibaba hasnât said

- **How many Token Plan credits a Qwen4-Max call uses,** for the full model or the preview.
- **How the preview differs from the full model,** and whether`qwen4-max-preview` will route to`qwen4-max` after November 25.
- **Which Qwen4 models will be open,** and when, and under what license.
- **The limits in QwenStudio and QwenWork.**
- **Whether Individual subscribersâ data trains its models,** beyond pointing to its service agreement.

Weâll update this memo when the preview retires or Alibaba releases the smaller Qwen4 models.

## 11.Questions.

- ### Is Qwen4 out?
- Yes. Alibaba released Qwen4-Max, the first full Qwen4 model, on November 16, 2026, China time, the evening of November 15 in the US, after a preview that began November 11. It says smaller Qwen4 models are âcoming to open source,â without names or dates.
- ### How much does Qwen4-Max cost?
- In QwenCloudâs international region itâs $1.50 per million input tokens and $4.50 per million output, with 1 million free tokens; OpenRouter charges the same. Itâs also on Alibabaâs Token Plan, from $6 a month or $3 for a first purchase, and on OpenCode Go, from $10 a month.
- ### Can I use Qwen4-Max in Claude Code?
- Yes, with an Alibaba key, paid to Alibaba rather than on a Claude plan. Alibabaâs guide puts qwen4-max in Claude Codeâs Opus slot through ANTHROPIC_DEFAULT_OPUS_MODEL. Set ANTHROPIC_MODEL to qwen4-max too, or choose Opus with /model, so the main model is Qwen4-Max.
- ### Is Qwen4-Max open source, and can I run it locally?
- No. Qwen4-Max runs only on Alibabaâs servers and the services that resell it, and Alibaba hasnât released its weights. At 2.4 trillion parameters, itâs far too big for a personal computer anyway. For a model you can run yourself, see how to run a coding agent on a local model.
- ### Is Qwen4-Max better than Claude or GPT?
- Not overall, on independent tests. Artificial Analysis scores it 43, against 59 for Claude Opus 5.5 and 55 for GPT-6.1 Sol. Alibabaâs own chart has it ahead on DeepSWE, at 75.7, but well behind Opus 5.5 on Terminal-Bench 4.0 and ProgramBench.
- ### What happens to Qwen4-Max-Preview?
- It stays on Token Plan, at as little as 20% of its standard credit rate, until 24:00 on November 25, China time, which is 10 a.m. Central, and then it retires. Alibaba hasnât said how it differs from qwen4-max, or whether its ID will point to qwen4-max afterward.
