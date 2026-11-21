---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-api-pricing-effort-levels-tested-computingforgeeks
title: "GPT-6 Astra API: Pricing, Effort Levels, Tested | ComputingForGeeks"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["astra", "gpt-6", "model", "tokens", "pricing", "effort", "chatgpt", "prompt"]
source: https://computingforgeeks.com/gpt-6-astra-released-features-benchmarks/
source_anchor: ""
source_lines: [1, 257]
section: "GPT_OpenAI"
---
GPT-6 Astra is still the most expensive model OpenAI sells, and seven weeks after launch it’s no longer the obvious one to call. GPT-6.1 Sol shipped on 29 September at a fifth of the token price. The planned GPT-6.1 Astra upgrade was cancelled over safety regressions, and since 7 October the default GPT-6 model in ChatGPT runs on Sol for paid plans. If you are paying $50 per million output tokens, you should know what that buys you now.

This refresh covers GPT-6 Astra pricing in the API and in ChatGPT, all five effort levels measured on one prompt, the benchmark claims, a run against GPT-6.1 Sol and Luna, prompt caching, and working API calls. Every API number below was re-run against `gpt-6-astra` from an Ubuntu 26.04 lab VM in October 2026, through OpenRouter with OpenAI as the serving provider at OpenAI’s list price.

## Check GPT-6 Astra specs and pricing

Astra is a single reasoning model with a 1.05M-token context window and five effort settings, and nothing about the spec sheet has changed since launch. The knowledge cutoff on the official model page is 30 April 2026, though the model still misreports it when asked (more on that below).

| Attribute | GPT-6 Astra | 
|---|---|
| API model ID | `gpt-6-astra` (no dated snapshot listed yet) | 
| Released | 3 September 2026 (limited preview), paid plans over the following days | 
| Context window | 1,050,000 tokens | 
| Max output | 128,000 tokens | 
| Knowledge cutoff | 30 April 2026 (per the model page) | 
| Reasoning effort | low, medium, high, xhigh, max | 
| Modalities | text and image in, text out | 
| Endpoints | Responses, Chat Completions, Batch (not Realtime or Assistants) | 
| Tools (Responses API) | web search, file search, code interpreter, hosted shell, apply patch, skills, computer use, MCP, tool search, image generation | 
| Fine-tuning | not supported | 
| Rate limits (RPM / TPM) | Build 5,000 / 1M, Launch 10,000 / 4M, Grow 15,000 / 40M | 

The price is where Astra stops being a drop-in upgrade. It lists at $10 per million input tokens and $50 per million output, five times the GPT-6.1 Sol rate. The long-context surcharge applies to the whole request once a prompt crosses 272K tokens:

| Token type (per 1M) | Standard | Prompt above 272K tokens | 
|---|---|---|
| Input | $10.00 | $20.00 | 
| Cached input (read) | $1.00 | $2.00 | 
| Cache write, 30 minute retention | $12.50 | $25.00 | 
| Cache write, 12 hour retention | $25.00 | 2x as well | 
| Output | $50.00 | $75.00 | 
| Batch and Flex | 50% of the rates above |  | 
| Fast mode | 2x the applicable rate, up to 2x the speed |  | 
| Data residency endpoints | 10% uplift |  | 

We checked the meter before trusting any cost figure in this guide. A 13-token prompt that returned 5 tokens billed $0.00038, which is exactly 13 tokens at $10 per million plus 5 at $50. If you priced a workload in September, your numbers still hold: the rates on the OpenAI pricing page haven’t moved since launch, and the Batch variant on OpenRouter bills at half, matching it.

## Compare GPT-6 Astra cost across ChatGPT plans

There is no “Astra” plan in ChatGPT. You get Astra through two doors, and which one your plan opens decides the real cost. In ChatGPT Work and Codex, Plus subscribers can pick Astra directly against a shared usage allowance. In the Chat tab, Astra only appears as GPT-6 Pro, the Pro thinking level, and that starts at the $100 Pro plan.

Since 7 October the everyday GPT-6 model in Chat runs on GPT-6 Sol for paid plans and Luna for Free and Go, so a Plus user chatting normally is not talking to Astra at all.

| Plan (monthly, US) | Astra in Work and Codex | GPT-6 Pro (Astra) in Chat | 
|---|---|---|
| Free ($0), Go ($8) | No, Luna only | No | 
| Plus ($20) | Yes, about 5 to 45 local messages per 5 hours, weekly cap on top | No | 
| Pro ($100, $200) | Yes, 5x or 20x Plus, no 5-hour window | Yes, separate weekly limit | 
| Pro ($500) | Yes, plus Astra Ultrafast | Yes | 
| Business (per seat) | Yes, same 5 to 45 range on Standard seats | Yes | 
| Enterprise, Edu | Admin enables it, credit-based | Admin enables it | 

The message ranges come from OpenAI’s Astra usage page and are estimates, not quotas. The same page puts GPT-6.1 Sol at 15 to 160 messages in the same window, so on Plus, Astra burns your allowance roughly three times faster.

Once you pass the allowance, credits bill Astra at 250 credits per million input tokens, 25 cached and 1,250 output, five times the GPT-6.1 Sol credit rate. Ultrafast, on Pro $500 and eligible Enterprise and Edu workspaces, draws included usage at 8x the standard rate and purchased credits at 6x. If you hit the Astra limit on Plus every week, price the $100 Pro tier against those credits before you buy more.

Two things trip people up. Astra in the Codex CLI needs version 0.153.0 or newer (install the current release rather than the floor), and the desktop app sometimes needs two update passes before Astra shows in the model picker. Our Codex CLI cheat sheet covers model switching and the `/status` command that shows what’s left of your allowance.

## Read the GPT-6 Astra benchmarks

OpenAI published Astra scores against GPT-5.6 Sol and, on several boards, against Anthropic’s Claude Fable 5.1 and Claude Opus 5. These are OpenAI’s numbers, not ours, and they’re the maximum score at any effort level:

| Benchmark | Astra | GPT-5.6 Sol | Opus 5 | Fable 5.1 | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 57.9% | 37.3% | 52.3% | 55.8% | 
| OSWorld 2.0 (computer use) | 72.6% | 65.7% | 70.2% | — | 
| ScreenSpot-Pro | 92.7% | 76.9% | — | — | 
| FrontierMath Tier 4 v2 | 97.6% | 83.0% | 73.2% | 87.8% | 
| GPQA Diamond | 96.0% | 94.6% | 93.7% | 93.7% | 
| ARC-AGI-3 (Provider Adapter harness) | 99.9% | 7.8% | 30.2% | — | 
| ExploitBench (cyber) | 100.0% | 78.5% | — | — | 
| SRE-Bench, first attempt | 88.0% | 55.9% | — | — | 

Read the ARC-AGI-3 row carefully before you quote it. ARC Prize got 99.9% with its Provider Adapter harness, which keeps the model’s opaque reasoning state between requests and compacts long conversations. With the Standard harness, where the model only carries its own notes forward, the same model scored 62.7% at `max` and 17.5% at `low`. That is still a record, but the number you see in headlines measures the scaffolding as much as the model. The jump is real on agentic, computer-use, cyber and math work, and it is much smaller on everyday questions, which is the whole argument about who should pay for it.

The bigger change since launch is the cheaper model catching up. OpenAI’s own GPT-6.1 Sol charts (29 September) put GPT-6.1 Sol at 75.2% on DeepSWE against an Astra best of 74.1%, at $0.65 per task against $4.43. Astra still leads where the work is hardest: 68.1% on Terminal-Bench Science against 57.0%, and 73.5% on offline OSWorld against 71.4% (those charts re-report Astra slightly above its 72.6% launch figure). Our GPT-6.1 Sol, GPT-6 Sol and Luna breakdown has the full chart tables, and the Claude Opus 5.5 release notes cover Anthropic’s answer from the same month.

## Pick a GPT-6 Astra effort level

GPT-6 Astra accepts five reasoning effort levels: `low`, `medium`, `high`, `xhigh` and `max`. It does not accept `none` or `minimal`, so you can’t switch reasoning off, though a trivial prompt may still spend zero reasoning tokens. On a real problem the thinking budget grows by roughly 2.2x to 2.8x per step. To see what that means for the bill, we sent the same debugging prompt to Astra at every level with a 16,000-token output ceiling:

| Effort | Latency | Reasoning tokens | Total output tokens | Cost | 
|---|---|---|---|---|
| low | 21.3s | 280 | 848 | $0.0430 | 
| medium | 31.9s | 785 | 1,299 | $0.0655 | 
| high | 54.5s | 2,070 | 2,557 | $0.1284 | 
| xhigh | 143.8s | 5,670 | 6,281 | $0.3146 | 
| max | 270.9s | 12,652 | 13,161 | $0.6586 | 

You can see the same run in the terminal capture below, read straight from the saved responses on the lab VM:

From `low` to `max` the cost rose 15x and the wait went from 21 seconds to four and a half minutes. The answers didn’t improve to match. All five named the same two core causes. All five also carried the caveat a senior engineer would add: a connection you forget to release inside one bounded asyncpg pool starves that pool, but it does not grow file descriptors without limit. The `max` answer was actually shorter than the `low` one.

Our September ladder told the same story with one extra lesson. At a 6,000-token ceiling the `max` run was cut off mid-answer, and here it needed 13,161 tokens. Set `max_tokens` well above 15K before you use that level at all.

OpenAI’s own guidance points the same way: its help center says Astra at low effort can outperform Sol at high effort. For most requests we would run Astra like this:

- `low` or`medium` as the default
- `high` for multi-step debugging and design reviews
- `xhigh` for long agentic runs you won’t watch
- `max` only for research problems, with a big output ceiling

If a `medium` answer misses something, check the prompt and the context it was given before raising the effort, because more thinking cannot supply a file the model never saw.

### Error: “Reasoning is mandatory for this endpoint and cannot be disabled”

Sending `"effort": "none"` to Astra fails with HTTP 400. Through OpenRouter, the error body reads:

`{"error":{"message":"Reasoning is mandatory for this endpoint and cannot be disabled.","code":400,"metadata":{"provider_name":null}}}`
That message is OpenRouter’s wording. Our GPT-6.1 Sol tests sent `none` straight to OpenAI’s API and got HTTP 400 with `"code": "unsupported_value"` and `"param": "reasoning.effort"`, and the Astra model page lists no `none` level either, so expect the same rejection when you call OpenAI directly. You hit this when a pipeline built for GPT-6 Sol or GPT-6 Luna, which both accept `none`, swaps in the Astra model ID. Map `none` to `low` before you change the model, or keep the zero-reasoning calls on GPT-6 Sol or GPT-6 Luna (GPT-6.1 Sol rejects `none` too) where they cost a fraction as much anyway.

## Compare GPT-6 Astra vs GPT-6.1 Sol

Benchmarks measure the ceiling, and day-to-day work sits closer to the floor. We gave Astra and the rest of the GPT-6 family the same prompt at `high` effort. It describes a FastAPI service using httpx and asyncpg that leaks file descriptors under load until it hits `EMFILE`, and asks for the three most likely root causes plus one exact Linux command to confirm each. Reasoning tokens are counted inside the output total and billed at the output rate, so the thinking you can’t read still lands on the invoice.

| Model | Latency | Reasoning tokens | Total output tokens | Cost | Bounded-pool caveat | 
|---|---|---|---|---|---|
| GPT-6 Astra | 73.1s | 2,541 | 3,065 | $0.1538 | Yes | 
| GPT-6.1 Sol | 49.2s | 2,043 | 2,514 | $0.0253 | Yes | 
| GPT-6 Sol | 20.7s | 2,070 | 2,365 | $0.0238 | No | 
| GPT-6 Luna | 58.2s | 5,696 | 6,154 | $0.0031 | No, listed it as a cause | 

The capture below is the same four runs as the table, summarised from the saved API responses:

You’d get the two causes that matter from any of them: a per-request `httpx.AsyncClient` or streamed response that is never closed, and asyncpg connections or pools created per request. The third pick split them. Astra and Luna both flagged that plain connection concurrency can exhaust the file descriptor limit with no leak at all, and Astra gave a `/proc/$PID/fd` count against the soft limit to tell the two apart. GPT-6.1 Sol and GPT-6 Sol went for unclosed upload or temp files.

Luna repeated a mistake the others warned against, listing an unreleased connection in a pool as a source of descriptor growth.

In September, when we ran the same kind of prompt against the GPT-5.6 family, Astra was the only model to add that bounded-pool caveat. In October, GPT-6.1 Sol added it too, for a sixth of the price and in two thirds of the time. That’s the real change since launch. For a question like this one, GPT-6.1 Sol is the default we would reach for. Astra earns its premium only where a missed caveat is expensive or the task runs long enough for the extra judgement to compound.

Our GPT-6 Astra vs Claude Opus 5 tests and the Kimi K3 vs GPT-6 Astra cost run cover the same question against other vendors.

## Cut the Astra bill with caching

Cached input reads cost $1 per million against $10 uncached, and that discount does most of the work in making Astra affordable. We sent the first 1,500 lines of the bash man page (20,284 tokens) as a system prompt twice, a few seconds apart, with a short question after it:

| Call | Input tokens | Cached tokens | Cost | 
|---|---|---|---|
| Cold (first send) | 20,284 | 0 | $0.25564 | 
| Warm (identical prefix) | 20,284 | 20,281 | $0.02241 | 

The warm call cost 91.2% less than the cold one, close to the 91.9% we measured in September on a different file. The capture shows both calls:

Look at the cold row again. It billed at the $12.50 cache-write rate, so the first send cost about 25% more than the same prompt would at plain input pricing ($0.2049). Caching pays off from the first reuse and costs you money on a prompt you never send twice.

Put the stable part of every request at the front (system prompt, schema, documents, tool definitions) and the varying question at the end, so the prefix matches byte for byte. Our context engineering guide covers how to order that context. The 12-hour retention write costs $25 per million, so only pay for it on a prefix you’ll replay many times across the day.

## Inspect the reasoning output

The API never returns Astra’s raw chain of thought. You get a readable summary and an encrypted blob, and only the summary is for you. We asked a one-line trick question at `high` effort and inspected the response: 52 reasoning tokens billed, the answer “1 minute.” (correct), and a `reasoning_details` array holding two items:

- `reasoning.summary` : 489 characters of first-person paraphrase
- `reasoning.encrypted` : a 2,197-character opaque token

The encrypted item exists so you can pass it back on the next turn and keep the model’s context, not so you can read it. The summary is a paraphrase written for display, and on this run it even muddled the arithmetic it described while the final answer came out right. On the native Responses API the readable part arrives as the summary on the reasoning output item.

OpenAI’s launch post is candid that this matters. In tests that asked the model to evade monitoring, Astra’s written reasoning was harder to monitor than GPT-5.6 Sol’s, which OpenAI attributes to it solving problems in fewer written steps. The Information reported that Astra uses a “recurrent depth” technique; OpenAI has described its use as limited without quantifying it.

For a coding assistant this changes nothing. If you audit an agent by reading its reasoning, or log why a compliance decision was made, you’re logging a summary the model chose to show, so build your audit trail around tool calls and outputs instead.

## Check the cyber safeguards

Astra scored 100% on ExploitBench and meets OpenAI’s Critical threshold for cybersecurity, so the public model ships with safeguards that refuse advanced offensive work such as proof-of-concept exploits. Defensive work goes through. In our September test, a question about SYN floods and the sysctl that mitigates them got a full answer. A request for a working exploit against a server we did not own was declined like this:

```
I can't write an exploit or help you break into a server you don't own or
have permission to test. If you're researching a CVE, I can walk through the
vulnerability class, the patch, and how to verify your own systems are fixed.
```
The refusal redirects to authorized testing rather than shutting the conversation down, which is what you want on a security team. Verified defenders who need vulnerability validation, malware analysis or detection engineering apply through OpenAI’s Daybreak program, which runs its own cyber models on separate credit rates. In the API, a request that trips a safety check is refused or stopped, so wrap agent loops with a retry path that surfaces the stop to a human instead of looping.

The safeguards also explain why there is no successor yet. On 28 September the Wall Street Journal reported, and TechCrunch followed, that OpenAI scrapped the planned October release of GPT-6.1 Astra after it regressed on internal safety tests. GPT-6 Astra stays on the price list, and GPT-6.1 Sol shipped in its place as the new mid-tier.

## Call the GPT-6 Astra API

Astra works on both the Responses and Chat Completions endpoints under the plain ID `gpt-6-astra`. On the Responses API the effort goes in a nested `reasoning` object; on Chat Completions it’s the flat `reasoning_effort` string. A direct call to OpenAI with the key in `OPENAI_API_KEY`:

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer ${OPENAI_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-6-astra",
    "input": "Summarize the risk of unclosed asyncpg pools in two sentences.",
    "reasoning": {"effort": "medium"}
  }'
```
Our October runs went through OpenRouter instead, which takes one key for every vendor and passes requests to OpenAI. The model ID gains an `openai/` prefix, and `jq` pulls the answer and the billed cost out of the response:

```
curl -s https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer ${OPENROUTER_API_KEY}" \
  -H "Content-Type: application/json" \
  -d '{"model":"openai/gpt-6-astra","reasoning":{"effort":"low"},"messages":[{"role":"user","content":"Give the systemctl command that shows why nginx failed to start. One line, no prose."}]}' \
  | jq '{model, text: .choices[0].message.content, usage: {prompt: .usage.prompt_tokens, completion: .usage.completion_tokens, reasoning: .usage.completion_tokens_details.reasoning_tokens, cost: .usage.cost}}'
```
A trivial prompt at `low` spent no reasoning tokens and cost a tenth of a cent:

```
{
  "model": "openai/gpt-6-astra",
  "text": "systemctl status nginx.service -l --no-pager",
  "usage": {
    "prompt": 25,
    "completion": 15,
    "reasoning": 0,
    "cost": 0.001
  }
}
```
From Python, the official `openai` package talks to either endpoint. Install it in a virtual environment first:

```
python3 -m venv ~/astra
~/astra/bin/pip install openai
```
Save the script as `astra_sdk.py`. It reads the key from the environment so it never lands in your code, and the `base_url` line is the only change you would drop to call OpenAI directly with `model="gpt-6-astra"`:

```
import os
from openai import OpenAI
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=os.environ["OPENROUTER_API_KEY"],
)
resp = client.chat.completions.create(
    model="openai/gpt-6-astra",
    reasoning_effort="medium",
    messages=[{"role": "user", "content": "Summarize the risk of unclosed asyncpg pools in two sentences."}],
)
print(resp.choices[0].message.content)
print(resp.usage.completion_tokens, resp.usage.completion_tokens_details.reasoning_tokens)
```
Run it with the venv’s interpreter:

`~/astra/bin/python astra_sdk.py`
The answer prints first, then the output and reasoning token counts:

```
Unclosed asyncpg pools can leave database connections and other resources open, potentially exhausting PostgreSQL’s connection limit and causing requests to fail. They can also complicate shutdown and trigger cleanup warnings or errors, so close pools with `await pool.close()` during application teardown.
57 0
```
One trap before you trust Astra on dates: ask it for its own knowledge cutoff and it still answers “My knowledge cutoff is June 2024.”, while the model page says 30 April 2026.

Pin anything date-sensitive to the model page or an external source, not to what the model says about itself. If you’d rather drive Astra from an agent in the terminal than from raw calls, our AI coding agent comparison covers the CLIs that accept a custom model ID.

## Is GPT-6 Astra worth $50 per million output tokens?

For most work, no, and that is truer in October than it was at launch. On our debugging prompt GPT-6.1 Sol now reaches the same answer with the same caveat for about a sixth of the cost. OpenAI’s own charts put it level with or ahead of Astra on agentic coding. Astra still leads on the hardest science and computer-use tasks, and it’s the model behind GPT-6 Pro in ChatGPT. Route the hard few percent of requests to it at `low` or `medium` and keep the stable prompt prefix cached. Send everything else to GPT-6.1 Sol, and budget for reasoning tokens you pay for but cannot fully read.

i think it is very good it can take people become better and better a

I think it is very good. It helps people become better and better.

Great work, thanks for sharing.

This is a wonderful article. I will share it with my friends.
