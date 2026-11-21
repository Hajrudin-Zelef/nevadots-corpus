---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-price-open-weights-api-and-vs-deepseek-apex-logic
title: "MiniMax M3.1 Flash: Price, Open Weights, API and vs DeepSeek | Apex Logic"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-23"]
keywords: ["minimax", "flash", "tokens", "deepseek", "october", "model", "input", "output"]
source: https://www.apex-logic.net/news/minimax-m3-1-flash-api-guide-2026
source_anchor: ""
source_lines: [1, 166]
section: "MiniMax"
---
*Updated 23 October 2026. MiniMax published the M3.1 Flash weights on Hugging Face today, four days after the API launch, and this guide covers the price, the license terms, API code and how it compares with DeepSeek V4.1 Flash.*

**MiniMax M3.1 Flash is MiniMax's new low-cost model for agents, coding and computer use, and as of 23 October 2026 the API model `MiniMax-M3.1-Flash` costs a flat $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens.** It launched on 19 October 2026 with a 1,000,000-token context window, and its model card lists text, image and video input. MiniMax says it cuts agent usage costs by 70% compared with its predecessor.

On 23 October MiniMax uploaded the weights to Hugging Face under the MiniMax Community License, which allows commercial use if you show a MiniMax credit and send MiniMax a one-time notice; products earning more than $20 million a year need written approval first. Against DeepSeek V4.1 Flash 1017, it matches DeepSeek's off-peak input and output prices at every hour, costs half as much for both during DeepSeek's peak hours and leads on MiniMax's entreprise automatisation test, while DeepSeek is faster, scores slightly higher on Artificial Analysis's index and has a simpler MIT license.

## MiniMax M3.1 Flash at a glance

From MiniMax's launch post, API docs and model card, checked on 23 October 2026:

| Item | MiniMax M3.1 Flash, as of 23 October 2026 | 
|---|---|
| API model name | `MiniMax-M3.1-Flash` | 
| Released | API on 19 October 2026; weights on Hugging Face on 23 October 2026 | 
| Size | About 428B total parameters, 23B active per token (mixture of experts) | 
| Context window | 1,000,000 tokens | 
| Input and output | Text, image and video in; text out | 
| Price per 1M tokens | $0.15 input, $0.60 output, $0.015 cached input, the same at every hour | 
| Thinking | Always on, with effort set to low, medium, high, xhigh or max; max if you set nothing | 
| API formats | Anthropic-compatible at `https://api.minimax.io/anthropic` (recommended) and OpenAI-compatible at`https://api.minimax.io/v1` | 
| Where to use it | MiniMax API, the M Plan subscription, MiniMax Code and MiniMax Agent | 
| Weights license | MiniMax Community License, with conditions for commercial use | 

MiniMax says an early version ran anonymously as "Space Bunny" and processed more than 200 trillion tokens before launch, and that a more capable Pro model will follow, with no date given.

## How much does MiniMax M3.1 Flash cost?

MiniMax's pricing page lists one rate for M3.1 Flash, with no separate long-prompt tier. Here it is next to the older MiniMax-M3, which carries a permanent 50% discount (standard tier, prices as of 23 October 2026):

| Model, per 1M tokens | Input | Output | Cached input | 
|---|---|---|---|
| MiniMax-M3.1-Flash | $0.15 | $0.60 | $0.015 | 
| MiniMax-M3, prompts up to 512K tokens | $0.30 | $1.20 | $0.06 | 
| MiniMax-M3, prompts over 512K tokens | $0.60 | $2.40 | $0.12 | 

### Example: a support chatbot for a month

An example with assumed numbers: 1,000 requests a day for 30 days. Each request sends 2,500 cached tokens of instructions and help content plus 500 new tokens, and gets 600 output tokens back, thinking included, at a low effort setting.

| Model, prices as of 23 October 2026 | Monthly cost | 
|---|---|
| MiniMax M3.1 Flash, any hour | About $14.18 | 
| DeepSeek V4.1 Flash 1017, off-peak | About $13.28 | 
| DeepSeek V4.1 Flash 1017, peak | About $26.55 | 

Either way the bill is small, and output tokens make up most of it. The number that changes the result is how many tokens each model writes for your task, which depends on the effort setting. For budgeting, see our guide on how to estimate and cut your LLM API bill. Artificial Analysis reported that M3.1 Flash produced 180M output tokens to run its Intelligence Index, which it calls very verbose, and that one index task costs about $0.22 on average.

## MiniMax M3.1 Flash vs DeepSeek V4.1 Flash 1017

Prices from both companies' pricing pages and speed figures from Artificial Analysis, as of 23 October 2026:

| Item | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 | 
|---|---|---|
| API model name | `MiniMax-M3.1-Flash` | `deepseek-flash` | 
| Input, per 1M tokens | $0.15 at all hours | $0.30 peak, $0.15 off-peak | 
| Output, per 1M tokens | $0.60 at all hours | $1.20 peak, $0.60 off-peak | 
| Cached input, per 1M tokens | $0.015 | $0.006 peak, $0.003 off-peak | 
| Context window | 1M tokens | 1M tokens, up to 384K output | 
| Thinking | Always on, five effort levels | On by default, with a non-thinking mode | 
| Output speed (Artificial Analysis) | 92.5 tokens per second | 206.1 tokens per second | 
| Intelligence Index (Artificial Analysis) | 43 | 45 | 
| Cost per index task (Artificial Analysis) | $0.22 | $0.26 | 
| Weights | Hugging Face, MiniMax Community License | Hugging Face, MIT license | 
| Extras | MiniMax Code, M Plan | Search and Fetch APIs at $0.75 per 1,000 successful calls | 

DeepSeek's peak hours are 01:00 to 04:00 and 06:00 to 10:00 UTC on weekdays, excluding Chinese public holidays. If your traffic lands in those hours, which cover UK and European mornings and much of the Asian working day, MiniMax costs half as much per token. Outside them the token prices match, and DeepSeek's cached input is cheaper. Artificial Analysis measured DeepSeek at more than twice the output speed, which matters for a chat window where people wait for the answer.

### Benchmarks: which model wins what

MiniMax's model card compares the two directly. These are MiniMax-run results:

| Test (MiniMax-reported) | M3.1 Flash | DeepSeek V4.1 Flash 1017 | 
|---|---|---|
| Automatisation-Bench v1.0.6 | 62.8 | 60.5 | 
| DeepSWE v1.1 | 73.7 | 77.0 | 
| Terminal Bench 4.0 | 38.1 | 42.3 | 
| NL2Repo-Bench | 61.2 | 68.2 | 
| BabyVision (with tools) | 91.0 | 92.5 | 
| HLE (without tools, text only) | 43.4 | 42.6 | 

So MiniMax leads on entreprise workflow automatisation and edges ahead on HLE, while DeepSeek leads on coding and terminal work. MiniMax also reports 76.2 on BankerToolBench, 61.7 on OSWorld 2.0 and 82.2 on CyberGym, with no DeepSeek 1017 figure for those. VentureBeat reported that on Artificial Analysis's AA-Omniscience test, M3.1 Flash answered 56% of questions correctly against 43% for DeepSeek, but that 93% of its non-correct answers were wrong answers rather than refusals or partial answers. For a chatbot that answers from your own documents, ground every reply in retrieved text and test how it handles questions it cannot answer.

## MiniMax M3.1 Flash vs M3

According to the model card, M3.1 Flash keeps M3's mixture-of-experts design with three changes: sparse attention throughout, replacing M3's first three full-attention layers; quantization-aware training; and DSpark speculative decoding, whose draft head ships with the weights. MiniMax claims online inference is nearly three times faster and agent usage costs 70% less. On list price, Flash's input and output rates are half of M3's discounted rates for prompts up to 512K tokens and a quarter above that. VentureBeat reported an Artificial Analysis Intelligence Index of 42 for Flash at launch, up from 29 for M3; the Artificial Analysis page showed 43 on 23 October.

## MiniMax M3.1 Flash open weights and license

The Hugging Face repository `MiniMaxAI/MiniMax-M3.1-Flash` went public on 23 October 2026 with MXFP8 weights split into 48 files, the DSpark draft head and a chat template. MiniMax recommends SGLang, vLLM or TokenSpeed for serving, with `temperature=1.0` and `top_p=0.95`. To download it:

`hf download MiniMaxAI/MiniMax-M3.1-Flash --local-dir MiniMax-M3.1-Flash`
Expect to need a multi-GPU server; MiniMax does not publish minimum hardware. The license is the part to read twice. The MiniMax Community License is free for non-commercial use. Commercial use, which it defines broadly to include paid products, commercial use of APIs that serve the model and deploying fine-tuned versions, comes with conditions:

- Prominently show "Built with MiniMax M3.1-Flash" on a related site web, interface, blog post, about page or product documentation.
- Send a one-time notice to MiniMax at api@minimax.io, or get written approval first if the product or service earns more than $20 million a year.
- Stay clear of the prohibited uses in the license appendix, which include military uses and harming minors.

DeepSeek's V4.1 Flash 1017 weights use the MIT license, which only asks you to keep the copyright and license text with copies, with no public credit and no notice to DeepSeek. Both can work for a small entreprise, but have a lawyer read the MiniMax terms before you ship a product on self-hosted weights or a third-party host. Our guide to open-weight LLMs vs API models covers the trade-offs of self-hosting.

## How to call the MiniMax M3.1 Flash API

MiniMax accepts Anthropic-style and OpenAI-style requests and recommends the Anthropic-compatible endpoint for full thinking support. If your app already uses the OpenAI SDK, change the base URL and model name. Create an API key on the MiniMax platform and store it in `MINIMAX_API_KEY`. The prompt below is an example:

```
import os
from openai import OpenAI
client = OpenAI(
    base_url="https://api.minimax.io/v1",
    api_key=os.environ["MINIMAX_API_KEY"],
)
response = client.chat.completions.create(
    model="MiniMax-M3.1-Flash",
    reasoning_effort="low",
    messages=[
        {"role": "system", "content": "You answer questions for a small online store. Keep answers under 80 words."},
        {"role": "user", "content": "Do you ship to Canada?"},
    ],
)
message = response.choices[0].message
print(message.content)
print(getattr(message, "reasoning_content", None))
print(response.usage)
```
Three things to know before you ship it:

- **Thinking cannot be turned off.** A request that disables thinking returns a 400 error. Lower the effort instead; with no effort set, M3.1 Flash uses`max` , the slowest and most token-hungry level.
- **Thinking comes back separately.** On the OpenAI-compatible endpoint it arrives in`reasoning_content` , and`content` holds only the answer, so you do not have to strip tags.
- **The effort field depends on the format:**`reasoning_effort` for OpenAI-style chat,`reasoning.effort` for the OpenAI Responses format and`output_config.effort` for Anthropic-style requests.

MiniMax also documents a Claude Code setup that points Claude Code at its Anthropic-compatible endpoint. For a production chatbot, keep the model provider in one configuration value so you can move between MiniMax, DeepSeek and others; our guide on how to build an AI agent covers the wider design.

## Which should a entreprise use?

- **Customer-facing chat where speed matters.** For an AI customer support chatbot, start with DeepSeek V4.1 Flash, which Artificial Analysis measured at more than twice the output speed and which can skip thinking; our DeepSeek V4.1 Flash 1017 guide has the setup. If you prefer a US provider, compare Claude Haiku 5.5 and GPT-6 Luna.
- **Back-office agents that work through SaaS tools.** Test M3.1 Flash. Its Automatisation-Bench and BankerToolBench results target this kind of work, and flat pricing helps if your jobs run during DeepSeek's peak hours.
- **Self-hosting.** DeepSeek's MIT license is simpler; MiniMax's works if you can meet the credit and notice terms.

Whichever you pick, run 30 to 50 of your real tasks through both, log tokens and cost per finished task, and check wrong answers by hand. For a full budget, see what an AI chatbot costs. If you want a support bot or workflow agent that can run on either model, tell us about the job.

## MiniMax M3.1 Flash FAQ

### How much does MiniMax M3.1 Flash cost?

As of 23 October 2026, MiniMax-M3.1-Flash costs $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens on the MiniMax API, at every hour and with no separate long-prompt tier. Thinking is always on, and higher effort levels produce more output tokens, so set a lower effort for simple tasks to keep the bill down.

### Is MiniMax M3.1 Flash open source?

Its weights are free to download, but the license is not a standard open-source license. MiniMax published the model on Hugging Face on 23 October 2026 under the MiniMax Community License. Non-commercial use is free. Commercial use requires a visible MiniMax credit and a one-time notice to MiniMax, and products earning more than $20 million a year need written approval first.

### Can I turn off thinking in MiniMax M3.1 Flash?

No. MiniMax's docs say thinking is always on, and a request that tries to disable it returns a 400 error saying the model requires adaptive thinking. You can lower the effort level to low or medium to cut latency and output tokens. If you set no effort at all, the API defaults to max, the slowest and most expensive setting for simple chat.

### Is MiniMax M3.1 Flash better than DeepSeek V4.1 Flash?

It depends on the job. In MiniMax's own table, M3.1 Flash leads on Automatisation-Bench, 62.8 against 60.5, while DeepSeek V4.1 Flash 1017 leads on DeepSWE, 77.0 against 73.7, and on Terminal Bench 4.0. Artificial Analysis rates DeepSeek slightly higher overall, 45 against 43, and about twice as fast, but MiniMax slightly cheaper per task. Test both on your own work.

### What is the MiniMax M3.1 Flash context window?

MiniMax-M3.1-Flash supports up to 1,000,000 tokens of context on the API, the same as MiniMax-M3 and DeepSeek V4.1 Flash. The model card notes that usable context and speed on self-hosted weights depend on GPU memory and the serving setup. Long prompts are still slow and costly, so retrieving only the relevant passages usually works better than sending whole document sets.

### Can I use MiniMax M3.1 Flash in Claude Code?

Yes. MiniMax documents a setup that points Claude Code at its Anthropic-compatible endpoint, sets MiniMax-M3.1-Flash as the model in the Claude Code settings file and recommends a 512K auto-compaction window. You tune thinking depth with Claude Code's effort setting, because thinking cannot be switched off. The guide sits under MiniMax's M Plan docs and uses a subscription key.

Loading comments...
