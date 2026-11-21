---
id: collect-261121-veille-ia/claude-anthropic/how-to-use-the-claude-sonnet-5-5-api-first-call-effort-thinking-tools-and-streaming
title: "How to Use the Claude Sonnet 5.5 API: First Call, Effort, Thinking, Tools, and Streaming"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2023-06-01"]
keywords: ["sonnet", "anthropic", "effort", "tokens", "thinking", "output", "claude", "messages"]
source: https://apidog.com/blog/how-to-use-claude-sonnet-5-5-api/
source_anchor: ""
source_lines: [1, 206]
section: "Claude_Anthropic"
---
To use the Claude Sonnet 5.5 API, send a POST request to `https://api.anthropic.com/v1/messages` with `"model": "claude-sonnet-5-5"`, your key in the `x-api-key` header, and `anthropic-version: 2023-06-01`. It costs $2 per million input tokens and $10 per million output tokens, reads up to 1M tokens of context, writes up to 128K, runs adaptive thinking by default, and defaults to `high` effort.

Anthropic released Sonnet 5.5 on September 28, 2026 (what is Claude Sonnet 5.5 covers specs and benchmarks). This guide walks through a first call in curl, Python and TypeScript, then effort, thinking, tools, streaming, refusals and rate limits. Moving Sonnet 5 code? The Sonnet 5.5 vs Sonnet 5 guide has every breaking change with before/after JSON. You can send each request below from Apidog and keep it as a saved test with assertions.

## Claude Sonnet 5.5 API at a glance

| Parameter | Sonnet 5.5 behavior | 
|---|---|
| Model ID | `claude-sonnet-5-5` (Bedrock:`anthropic.claude-sonnet-5-5` ) | 
| Price per MTok | $2 input, $10 output, $0.20 cache reads; Batch $1/$5 | 
| Context / output | 1M / 128K; 300K on Batch with the `output-300k-2026-03-24` beta | 
| `output_config.effort` | `low` ,`medium` ,`high` (default),`xhigh` ,`max` | 
| `thinking.type` | `adaptive` (default when omitted) or`between_tools` ;`disabled` returns 400 | 
| `thinking.display` | `omitted` (default),`summarized` ,`updates` (beta) | 
| `tool_choice` | `auto` or`none` ;`any` and`tool` return 400 | 
| `temperature` ,`top_p` ,`top_k` | Non-default values return 400 | 
| Minimum cacheable prompt | 512 tokens (1,024 on Sonnet 5) | 
| `max_tokens` for agentic coding | 128,000, with streaming | 

Sources: the Sonnet 5.5 model page and the migration guide.

## Claude Sonnet 5.5 API example: your first call

Create a key (the Anthropic API key guide walks through it) and export it as `ANTHROPIC_API_KEY` rather than hardcoding it. Then send this:

```
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-5-5",
    "max_tokens": 4096,
    "output_config": {"effort": "medium"},
    "messages": [{"role": "user", "content": "Explain idempotency keys in two sentences."}]
  }'
```
The Python SDK reads `ANTHROPIC_API_KEY` from the environment:

```
import anthropic
client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-5-5",
    max_tokens=4096,
    output_config={"effort": "medium"},
    messages=[{"role": "user", "content": "Explain idempotency keys in two sentences."}],
)
print(response.stop_reason)
for block in response.content:
    if block.type == "text":
        print(block.text)
```
TypeScript works the same way:

```
import Anthropic from "@anthropic-ai/sdk";
const client = new Anthropic();
const response = await client.messages.create({
  model: "claude-sonnet-5-5",
  max_tokens: 4096,
  output_config: { effort: "medium" },
  messages: [{ role: "user", content: "Explain idempotency keys in two sentences." }],
});
for (const block of response.content) {
  if (block.type === "text") console.log(block.text);
}
```
Read content blocks by `type`. Thinking is on by default, so a response can open with a `thinking` block, and code that reads `content[0].text` breaks. Thinking tokens are billed as output and count toward `max_tokens` even when their text is hidden, so leave headroom above the reply you expect.

## Choose an effort level

Effort, set in `output_config.effort`, is your main cost and quality dial. Anthropic recalibrated the levels for Sonnet 5.5, so a Sonnet 5 setting doesn’t carry over; run a fresh sweep on your own evals. The prompting guide suggests these starting points:

| Workload | Start at | 
|---|---|
| General work | `high` (the API default) | 
| Agentic coding, well-specified tasks | `medium` , moving to`high` for harder or longer ones | 
| Chat and latency-sensitive calls | `medium` or`low` | 
| Hard tasks where your evals show a measured gain | `xhigh` or`max` | 

The spread is wide. On Anthropic’s own Terminal-Bench 4.0 runs, Sonnet 5.5 scored 43.0% at `high` for $1.94 per attempt and 70.6% at `max` for $12.54. The Sonnet 5.5 pricing breakdown works through cost per request.

Plan for three behaviors. From `medium` up, the model thinks before almost every reply, even a greeting, and prompting it to think less isn’t reliable: lower the effort instead. At `low` and `medium` it tends to check in early on long agentic tasks. And changing top-level effort between requests invalidates the prompt cache. To switch levels mid-conversation and keep the cache, use per-message effort (beta, header `anthropic-beta: mid-conversation-output-config-2026-07-01`): add a `role: "system"` message with empty `content` and the new `output_config.effort`.

## Control thinking: adaptive or between_tools

Omit the `thinking` field and Sonnet 5.5 runs adaptive thinking. It rejects `{"type": "disabled"}` with a 400. To turn off up-front thinking, send `between_tools`, the lowest setting:

```
{
  "model": "claude-sonnet-5-5",
  "max_tokens": 16000,
  "thinking": {"type": "between_tools"},
  "output_config": {"effort": "high"},
  "messages": [{"role": "user", "content": "..."}]
}
```
Rules for Sonnet 5.5 `between_tools`:

- It works only at `low` ,`medium` or`high` . At`xhigh` or`max` it returns 400.
- It takes no other field. Adding `display` ,`budget_tokens` or`block_binding` returns 400.
- It needs no beta header and works on every platform.
- Effort can’t change mid-conversation while it’s set.
- SDK versions that don’t define it fail type checking, so update your SDK.

Under adaptive thinking, `display` decides what thinking blocks contain. The default, `omitted`, returns each `thinking` block with an empty `thinking` field plus a `signature`. `summarized` returns readable summaries. `updates` (beta, header `thinking-display-updates-2026-08-18`) returns only progress updates as text.

Progress updates are the change most likely to confuse a UI. Sonnet 5.5 puts notes longer than a sentence or two, written between tool calls, into their own `thinking` blocks instead of `text`. Under the default `omitted` those blocks are empty, so an agent interface that used to narrate its steps goes quiet. Set `display: "updates"` or `"summarized"`, or run `between_tools`, which returns the notes with text. Render each non-empty `thinking` block before the `tool_use` block that follows it. Asking for reasoning in the reply text invites a `reasoning_extraction` refusal, so read these blocks instead.

## Use tools without forced tool_choice

Forced tool use is gone. A `tool_choice` of `{"type": "any"}` or `{"type": "tool", ...}` returns a 400 with this message, on the token-counting endpoint too:

```
tool_choice: type "tool" and "any" are not supported for this model.
```
Send `auto`, mark the tool `strict: true` so its input matches the schema, and tell the model in the prompt when to call it:

```
{
  "model": "claude-sonnet-5-5",
  "max_tokens": 1024,
  "tools": [{
    "name": "get_weather",
    "description": "Get the current weather for a city",
    "input_schema": {
      "type": "object",
      "properties": {"location": {"type": "string"}},
      "required": ["location"],
      "additionalProperties": false
    },
    "strict": true
  }],
  "tool_choice": {"type": "auto"},
  "messages": [{"role": "user", "content": "What's the weather in Paris? Use the get_weather tool."}]
}
```
A request can carry at most 20 strict tools, and strict schemas need `additionalProperties: false` on every object. On Amazon Bedrock, strict tools aren’t available for Sonnet 5.5: send `auto` without `strict` and validate the input in your code.

Two loop details matter. Pass every `thinking` block back unchanged with its `tool_use` block, empty ones included. And expect the occasional case slip, such as `bash` for a tool declared as `Bash`. The prompting guide suggests accepting unambiguous matches, or returning a `tool_result` with `is_error: true` that states the exact name.

## Stream responses

Add `"stream": true` to the body, or use the SDK’s stream helper. For agentic coding, the prompting guide recommends `max_tokens` of 128,000 with streaming:

```
with client.messages.stream(
    model="claude-sonnet-5-5",
    max_tokens=128000,
    output_config={"effort": "medium"},
    messages=[{"role": "user", "content": "Review this diff for bugs: ..."}],
) as stream:
    for event in stream:
        if event.type == "content_block_delta" and event.delta.type == "text_delta":
            print(event.delta.text, end="", flush=True)
    final = stream.get_final_message()
```
Server-sent events arrive as `message_start`, then `content_block_start`, `content_block_delta` and `content_block_stop` for each block, then `message_delta` (carrying `stop_reason`) and `message_stop`. Under `omitted`, a thinking block streams one empty `thinking_delta` and a `signature_delta`, then text begins. Expect a pause of several seconds before a progress-update block opens.

`stream.get_final_message()` (TypeScript: `stream.finalMessage()`) rebuilds complete blocks with their signatures. Append that content to history as the assistant turn, unchanged, and keep history append-only. Sonnet 5.5 signs each thinking block over the conversation before it, so on accounts created on or after August 31, 2026 (00:00 UTC), replaying a block after editing earlier history returns 400. Blocks are also tied to the account that produced them.

## Handle refusals and fallback

A refusal isn’t an error. You get HTTP 200 with `stop_reason: "refusal"` and a `stop_details` object whose `category` is `cyber`, `bio`, `frontier_llm`, `reasoning_extraction` or `general_harms`, plus an `explanation`. Display the explanation rather than parsing it; its wording isn’t stable. Branch on `stop_reason` before you read `content`.

Server-side fallback is opt-in. Add `"fallbacks": "default"` and the `anthropic-beta: server-side-fallback-2026-07-01` header (beta, Claude API only), and the API retries `cyber` and `frontier_llm` declines on Sonnet 5. The other three categories aren’t retried. The response’s `model` field names the model that served it, and a `fallback` content block marks the handoff.

## Rate limits

Sonnet 5.5 has its own rate limit, separate from Sonnet 5’s. The rate limits page lists four tiers:

| Tier | Requests/min | Input tokens/min | Output tokens/min | 
|---|---|---|---|
| Start | 1,000 | 2,000,000 | 400,000 | 
| Build | 5,000 | 5,000,000 | 1,000,000 | 
| Scale | 10,000 | 10,000,000 | 2,000,000 | 
| Custom | Contact sales | Contact sales | Contact sales | 

For 429 handling and backoff, see the rate limit exceeded guide.

## Test the Claude Sonnet 5.5 API in Apidog

Saved requests make effort comparisons and stream debugging repeatable. Here’s the setup in Apidog:

1. Create an environment and add `ANTHROPIC_API_KEY` as a variable. Reference it as`{{ANTHROPIC_API_KEY}}` in the`x-api-key` header, next to`anthropic-version` and`content-type` .
2. Create a POST request to `https://api.anthropic.com/v1/messages` , paste the first-call body, and save it.
3. Add assertions: status is 200, `$.stop_reason` equals`end_turn` ,`$.usage.output_tokens` is greater than 0, and`$.content[*].type` contains`text` . A refusal now fails the test instead of passing silently.
4. Duplicate the request with `"stream": true` . Apidog shows the`text/event-stream` response event by event, so you can watch the empty`thinking_delta` , the`signature_delta` and the text arrive in order.
5. Clone it again with `"model": "claude-sonnet-5"` and keep the pair in one folder: same prompt, two models,`usage` side by side.

For broader patterns, see testing LLM applications and testing AI agent APIs.

## FAQ

**What is the Claude Sonnet 5.5 model ID?** `claude-sonnet-5-5`, with no date suffix, on the Claude API, Google Cloud, Microsoft Foundry and Claude Platform on AWS. On Amazon Bedrock it’s `anthropic.claude-sonnet-5-5`.

**Can I turn thinking off completely?** No. `disabled` returns 400. `between_tools` is the lowest setting: no up-front thinking, at `low`, `medium` or `high` effort.

**Why does my Sonnet 5 request return 400 on Sonnet 5.5?** Check for `thinking.type: "disabled"` and a forced `tool_choice` first. The Sonnet 5.5 vs Sonnet 5 guide covers all five breaking changes and their fixes.

**Is there a free Claude Sonnet 5.5 API?** Anthropic’s API is prepaid, and no official page lists a free signup credit. A Claude chat plan doesn’t include API access either. The free API guide covers credit programs and the cheapest paid path.

## Next step

Send the first-call request at `medium`, then rerun it at `high` and compare `usage.output_tokens` and answer quality on a prompt from your own workload. Download Apidog to save both runs with assertions. If you’d rather work from the terminal, see Claude Sonnet 5.5 in Claude Code.
