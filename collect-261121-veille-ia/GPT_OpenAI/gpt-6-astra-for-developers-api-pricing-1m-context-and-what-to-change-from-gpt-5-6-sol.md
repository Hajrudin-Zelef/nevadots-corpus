---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-for-developers-api-pricing-1m-context-and-what-to-change-from-gpt-5-6-sol
title: "GPT-6 Astra for developers: API, pricing, 1M context, and what to change from GPT-5.6 Sol"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "astra", "model", "gpt-6", "gpt-6-astra", "context", "responses", "text"]
source: https://apidog.com/blog/gpt-6-astra-api/
source_anchor: ""
source_lines: [1, 161]
section: "GPT_OpenAI"
---
OpenAI released GPT-6 Astra on September 3, 2026, first to a limited set of organizations and then, over the following days, to every ChatGPT Plus, Pro, Business, and Enterprise user, the OpenAI API, Microsoft Azure, and AWS Bedrock. The model ID is `gpt-6-astra`, it carries a 1,050,000-token context window, and OpenAI calls it “the best model for software engineering to date.” It is also the first model OpenAI has rated Critical for cybersecurity capability, which shapes how it behaves in the API.

This is the practical guide. It covers the model ID and endpoints, your first request, the five reasoning effort levels, the full price table including the long-context and Fast mode rates, the changes that break a GPT-5.6 Sol integration, and whether the 2.5x price over Sol’s promotional rate buys enough. The OpenAI model page is the source of record for the numbers below. For what it feels like to work with, read our two-day hands-on; this piece stays on the API.

## TL;DR

- Model ID `gpt-6-astra` , one snapshot. Chat Completions, Responses, and Batch. No Realtime, no Assistants, no fine-tuning. Tool calling needs the Responses API.
- 1,050,000-token context, 128,000 max output, knowledge cutoff April 30, 2026. Text and image in, text out.
- Standard pricing: $10 input, $1 cached read, $12.50 cache write, $50 output per million tokens. Prompts over 272K input tokens bill at $20 / $2 / $75. Batch and Flex are half price. Fast mode is double.
- Reasoning effort: `low` ,`medium` ,`high` ,`xhigh` ,`max` .`none` and`minimal` are gone.
- Breaking from Sol: `temperature` ,`top_p` , and logprobs are removed;`prompt_cache_retention` becomes`prompt_cache_options.ttl` .
- GPT-5.6 Sol is $4 / $20 on promotion through at least November 21, 2026, so Astra costs 2.5x on both sides.

## GPT-6 Astra at a glance

| Item | Value | 
|---|---|
| Model ID | `gpt-6-astra` | 
| Context window | 1,050,000 tokens | 
| Max output | 128,000 tokens | 
| Knowledge cutoff | April 30, 2026 | 
| Modalities | Input: text, image. Output: text | 
| Endpoints | Chat Completions, Responses, Batch | 
| Not supported | Realtime, Assistants, fine-tuning | 
| Features | Streaming, structured outputs, function calling, file search, web search, prompt caching, image input | 
| Built-in tools | Computer use, web search, file search, code interpreter, image generation | 
| Reasoning effort | `low` ,`medium` ,`high` ,`xhigh` ,`max` | 
| Rate limits (Tier 5) | 15,000 RPM, 40,000,000 TPM | 
| Also available on | Microsoft Azure, AWS Bedrock; OpenRouter as `openai/gpt-6-astra` | 
| Data handling | Zero Data Retention for eligible API customers | 

Enterprise workspaces get Astra switched off by default; an administrator has to enable it. On ChatGPT, Astra usage counts against the existing subscription allowance, and Pro, Business, and Enterprise plans also receive GPT-6 Astra Pro.

## Your first request

The Responses API is the primary surface, and it is required for tools. A minimal Python call:

```
from openai import OpenAI
client = OpenAI()
response = client.responses.create(
    model="gpt-6-astra",
    reasoning={"effort": "medium"},
    input=[
        {"role": "developer", "content": "You are a senior API engineer. Bias towards action. Ask only when the answer would change the result."},
        {"role": "user", "content": "Review this OpenAPI operation for auth and validation gaps, then propose three test cases."},
    ],
)
print(response.output_text)
print(response.usage)
```
The same request in curl:

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-6-astra",
    "reasoning": {"effort": "medium"},
    "input": "Review this OpenAPI operation for auth and validation gaps, then propose three test cases."
  }'
```
Chat Completions still works for plain text: swap the endpoint and the message shape, and drop any `temperature` or `top_p` fields, which OpenAI’s guidance says to remove. The moment you need function calling or a built-in tool, move to Responses. Our Responses API guide covers the request shape in depth.

The developer message matters more than it used to. OpenAI’s model guidance says Astra “asks for clarification more readily” than its predecessors and recommends telling it to bias towards action. It is also “more sensitive to instructions contained in skills” and other attached files, so state that user instructions win when they conflict. And it defaults to lists and tables; if you want prose, ask for it.

## Reasoning effort: five levels, no “none”

GPT-5.6 exposed six effort levels from `none` to `max`. Astra exposes five: `low`, `medium`, `high`, `xhigh`, and `max`. OpenAI’s migration advice is direct: if you run `none` or `minimal` today, move to `low` and evaluate. Everything else keeps its current setting.

That removal changes the cheap end of your workload. Luna at `none` was the fast classical-completion option in the GPT-5.6 family, and there is no Astra equivalent. Astra is a reasoning model at every setting, which is why routing the mechanical work to GPT-5.6 Terra or Luna and reserving Astra for the hard calls remains the sane pattern.

At the other end, `max` is where the launch benchmarks were run (“evaluation scores are the maximum at any effort”) and where Artificial Analysis measured a time to first token north of seven minutes on its max-effort run. Budget for latency before you budget for tokens.

## What GPT-6 Astra costs

Per million tokens, from OpenAI’s pricing page:

| Tier | Input | Cached input | Output | 
|---|---|---|---|
| Standard | $10.00 | $1.00 | $50.00 | 
| Standard, long context (over 272K input) | $20.00 | $2.00 | $75.00 | 
| Batch / Flex | $5.00 | $0.50 | $25.00 | 
| Batch, long context | $10.00 | $1.00 | $37.50 | 
| Fast mode | $20.00 | $2.00 | $100.00 | 
| Fast mode, long context | $40.00 | $4.00 | $150.00 | 

Cache writes bill at $12.50 per million. Fast mode delivers “up to 2x the speed of Standard processing at 2x the Standard price.”

For comparison, the current GPT-5.6 family:

| Model | Input | Cached input | Output | 
|---|---|---|---|
| GPT-5.6 Sol (promo through at least Nov 21, 2026) | $4.00 | $0.40 | $20.00 | 
| GPT-5.6 Terra | $2.00 | $0.20 | $12.00 | 
| GPT-5.6 Luna | $0.20 | $0.02 | $1.20 | 

Sol’s list price is $5 and $30; the $4 and $20 rate has run since August 21 and OpenAI says it holds at least through November 21. Against that, Astra is 2.5x on input and 2.5x on output. Against Sol’s list price it is 2x and 1.67x.

A worked example. An agent run that reads a 200,000-token codebase snapshot once, reuses it as a cached prefix across 30 calls, and writes 60,000 tokens of output in total:

- Astra: $2.00 for the first read, then 29 cached reads at $0.20 each ($5.80), plus $3.00 of output. About $10.80.
- Sol on promotion: $0.80 for the first read, $2.32 for the cached reads, $1.20 of output. About $4.32.

The ratio holds at 2.5x. What OpenAI argues, and what you should measure, is that Astra finishes in fewer calls and fewer output tokens. On Terminal-Bench 4.0 it claims roughly 9% lower cost per task than Sol despite the higher rate, and about 65% fewer output tokens than Claude Opus 5 on Agents’ Last Exam. If those hold for your workload, the per-task bill can come out level. If they don’t, you pay 2.5x.

Two levers keep the bill down. The 272K threshold is the one to watch: a prompt that crosses it doubles the input and cache rates, so trim tool outputs and cache the static prefix. And Batch halves everything for work that can wait.

## Migrating from GPT-5.6 Sol: what breaks

OpenAI published a short list, and it is worth taking literally.

1. **Sampling parameters are gone.** Remove`temperature` ,`top_p` , and`top_logprobs` . On Chat Completions also remove`logprobs` ; on Responses remove`message.output_text.logprobs` from`include` . OpenAI’s guidance is to drop them instead of relying on the API ignoring them, so grep your client code.
2. **Effort floor is `low`.** Any`none` or`minimal` setting moves to`low` .
3. **Cache retention changed shape.** Replace`prompt_cache_retention` with`prompt_cache_options.ttl` set to`"30m"` . The explicit caching mode you set up for GPT-5.6 carries over otherwise.
4. **Tools need Responses.** Chat Completions is supported for text, but function calling and the built-in tools live on the Responses API.
5. **Prompts want less hedging from you and more from it.** Add the bias-towards-action line, declare that user instructions take precedence over skill files, and request prose where your UI expects prose.

Nothing in the list touches structured outputs or function definitions, so a schema that worked on Sol works on Astra. The behavioral change most teams notice first is the asking: a Sol prompt that ran to completion may now stop with a clarifying question. Answer it in the developer message up front and it stops asking.

## Is it worth 2.5x Sol?

For agentic and long-context work, the launch numbers say yes. All figures below are OpenAI-run, at the best effort for each model:

| Benchmark | GPT-6 Astra | GPT-5.6 Sol | Claude Fable 5.1 | 
|---|---|---|---|
| Terminal-Bench 4.0 | 57.9% | 37.3% | 55.8% | 
| DeepSWE v1.1 | 74.1% | 72.7% | 67.4% | 
| FrontierCode 1.1 Extended | 64.5% | 60.6% | 63.6% | 
| Internal database migration tasks | 63.9% | 42.7% | 57.8% | 
| OSWorld 2.0 | 72.6% | 65.7% | - | 
| MRCR v2 8-needle, 512K to 1M | 96.3% | 73.8% | - | 
| GPQA Diamond | 96.0% | 94.6% | 93.7% | 
| Humanity’s Last Exam (with tools) | 57.2% | - | 65.0% | 

The Terminal-Bench and database-migration gaps are the ones that matter for developers: 20 points over Sol on agentic terminal work, and a long-context retrieval score that makes the 1M window usable instead of nominal. The DeepSWE gap is small, and Claude Fable 5.1 still leads on Humanity’s Last Exam by nearly eight points, so this is not a clean sweep. On Artificial Analysis’ independent index, Astra sits second among 202 models. Our Fable 5.1 benchmark breakdown has the other side of that comparison.

Where Sol keeps its job: short, high-volume calls where the effort floor and the 2.5x rate dominate, and anything that needs `none`-style latency. Where Astra earns it: long agent runs, whole-repository context, computer use, and any task where Sol’s failure mode was drifting or giving up.

## Test the swap before you ship it

The migration is small enough to do in an afternoon and consequential enough to measure. In Apidog, keep the model ID as an environment variable so the same saved request runs against `gpt-5.6-sol` and `gpt-6-astra` with one switch. Add assertions on `usage.input_tokens`, `usage.output_tokens`, and the cached-token count, then send a representative task set through both and compare the totals; that is the 2.5x question answered with your own traffic instead of a launch chart.

Two more checks belong in the same project. Send a request that still carries `temperature` and record what comes back, so you learn the behavior in a test instead of in production. And assert that a long prompt stays under 272K input tokens, because crossing that line silently doubles the rate. Schedule the set as a regression and Download Apidog if you don’t have it yet; the GPT-5.6 API guide shows the same rig on the previous generation.

## FAQ

**What is the GPT-6 Astra model ID?** `gpt-6-astra`, with a single snapshot. It is also exposed through Azure and AWS Bedrock, and on OpenRouter as `openai/gpt-6-astra`.

**Does GPT-6 Astra support fine-tuning or the Realtime API?** No. The model page lists Chat Completions, Responses, and Batch as supported, with Realtime, Assistants, and fine-tuning unsupported.

**What is the context window and max output?** 1,050,000 input tokens and 128,000 output tokens. Prompts above 272K input tokens are billed at the long-context rate.

**Why did my request fail after switching from Sol?** Most likely a leftover `temperature` or `top_p`, a `none` effort level, or `prompt_cache_retention`. Astra removed all three; see the migration list above.

**Can a request stop on its own?** Yes. OpenAI runs a misalignment monitor on tool-using requests, and in the API a flagged task stops instead of pausing for review. Long agent runs are the most exposed, so make them resumable. The Critical cyber threshold post explains the safeguards behind that behavior.

## What to do this week

Move one agent workload to `gpt-6-astra` on the Responses API, strip the sampling parameters, set effort to `medium`, and measure tokens and wall time against Sol on the same inputs. If the per-task cost comes out level or better, migrate the rest. If it doesn’t, you have a number, which is more than most teams will have by Friday.
