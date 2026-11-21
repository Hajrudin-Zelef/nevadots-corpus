---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-the-complete-developers-guide-cheat-sheet
title: "Claude Fable 5.2: The Complete Developer’s Guide & Cheat Sheet"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "claude", "anthropic", "output", "tokens", "model", "price", "fast"]
source: https://medium.com/@rajadurai80/claude-fable-5-2-the-complete-developers-guide-cheat-sheet-5ffbeb68e6a1
source_anchor: ""
source_lines: [1, 345]
section: "Claude_Anthropic"
---
# Claude Fable 5.2: The Complete Developer’s Guide & Cheat Sheet

Anthropic released Claude Fable 5.2 on November 9, 2026, and it is the most capable Claude model anyone can sign up and use today. It costs 20% less than Fable 5.1, writes up to 30% faster, and keeps the 1M-token context window and 128K output ceiling.

Fable 5.2 is the public face of the Mythos tier. It shares its underlying model with Claude Mythos 5.2 but adds safeguards for cybersecurity and biology. That one design choice explains most of what is different about building with it: always-on thinking, a high default effort, and a refusal path you should plan for.

This guide covers what changed, a copy-paste API cheat sheet, the effort-vs-cost trade-off, and when Fable is worth paying for over Opus 5.5.

## At a glance

Fable 5.2 keeps Fable’s 1M context and 128K output, and cuts the token price by 20% to $8 / $40 per million (model page).

- **API model ID:**`claude-fable-5-2` (Bedrock:`anthropic.claude-fable-5-2` )
- **Released:** November 9, 2026 (retires no sooner than November 9, 2027)
- **Context window / max output:** 1M tokens / 128K tokens
- **Price:** $8 input / $40 output per million tokens (Fable 5.1 was $10 / $50)
- **Prompt caching:** $10 (5-min write), $16 (1-hour write), $0.20 (read) per million tokens
- **Batch API:** 50% off, so $4 / $20
- **Fast mode:** up to 2.5x output speed at $16 / $80
- **Thinking:** adaptive, always on
- **Default effort:**`high` on the API (five levels: low, medium, high, xhigh, max)
- **Input → output:** text and images → text
- **Knowledge cutoff:** June 2026
- **Data retention:** 30 days required; no zero data retention unless Anthropic authorizes it
- **Platforms:** Claude API, Claude Platform on AWS, Amazon Bedrock, Google Cloud, Microsoft Foundry

## Fable vs Mythos vs Opus: where it sits

Fable 5.2 and Mythos 5.2 are the same model at the same price; Fable adds cybersecurity and biology safeguards so Anthropic can offer it to everyone (what’s new).

**Claude Fable 5.2** (`claude-fable-5-2`) is open to all customers. Its safety classifiers can decline a request with `stop_reason: "refusal"`. It costs $8 / $40 per million tokens, defaults to `high` effort, and is built for the hardest reasoning and long-horizon agents.

**Claude Mythos 5.2** (`claude-mythos-5-2`) is the same model without those classifiers, limited to organizations verified through Anthropic's programs such as Cyber Verification. Same price, same `high` default, aimed at verified security and life-sciences work.

**Claude Opus 5.5** (`claude-opus-5-5`) is open to all customers at half the price, $4 / $20, and defaults to `medium` effort. It is Anthropic's recommended starting point for most workloads.

Anthropic’s own guidance: start with Opus and move to Fable 5.2 when your evals on Opus at higher effort still fall short. Anthropic reports about 55% fewer blocks than Fable 5.1, based on early testing in its own apps (MacRumors).

## What’s actually new in Fable 5.2

The headline changes are a lower price, a new fast mode, faster output, and fewer refusals; the API surface is mostly the same as Fable 5.1, with four breaking changes covered under Gotchas.

## 1. Lower price, same limits

Input and output drop from $10 / $50 to $8 / $40 per million tokens. Cache reads stay at 0.025x base input ($0.20), and batch halves everything to $4 / $20. The 1M context is billed at the standard rate across the full window.

## 2. Fast mode (new for Fable)

Set `speed="fast"` for up to 2.5x higher output tokens per second at $16 / $80 per MTok. It speeds up generation, not time to first token, and it does not work with the Batch API or Priority Tier (fast mode docs).

`import anthropic`
client = anthropic.Anthropic()response = client.messages.create(

model="claude-fable-5-2",

max_tokens=8192,

speed="fast",

messages=[{"role": "user", "content": "Refactor this module to use dependency injection"}],

)

print(response.usage.speed) # "fast" or "standard"

On Microsoft Foundry, fast mode is a separate deployment, `claude-fable-5-2-fast`, and you leave `speed` out.

## 3. Effort is the main dial

Adaptive thinking is always on and cannot be disabled, so `output_config.effort` is how you trade quality for cost and latency. Five levels are available; the API default is `high` (effort docs).

```
response = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=64000,
    output_config={"effort": "medium"},  # low | medium | high | xhigh | max
    messages=[{"role": "user", "content": "Plan the migration"}],
)
```
## 4. Change effort mid-conversation without losing the cache (beta)

A `role: "system"` message with empty content and its own `output_config` changes effort from the next user turn while keeping the cached prefix.

```
response = client.beta.messages.create(
    model="claude-fable-5-2",
    max_tokens=16000,
    output_config={"effort": "high"},
    messages=[
        {"role": "user", "content": "Plan a migration from SQLite to PostgreSQL."},
        {"role": "assistant", "content": "1. Export... 2. Create schema... 3. Import and verify."},
        {"role": "system", "content": [], "output_config": {"effort": "low"}},
        {"role": "user", "content": "Summarize the plan in one sentence."},
    ],
    betas=["mid-conversation-output-config-2026-07-01"],
)
```
## 5. Refusals you can route around

A declined request is an HTTP 200 with `stop_reason: "refusal"` and a `stop_details.category` such as `cyber` or `bio`. Server-side fallback (beta) retries it on the model Anthropic recommends for that category; for Fable 5.2 the permitted targets are Claude Opus 4.8 and Claude Opus 5 (refusals and fallback).

```
response = client.beta.messages.create(
    model="claude-fable-5-2",
    max_tokens=4096,
    messages=[{"role": "user", "content": "..."}],
    fallbacks="default",
    betas=["server-side-fallback-2026-07-01"],
)
print(response.model)  # which model actually answered
```
## 6. New beta tools for long agent runs

- **Compaction on demand** (`compact-2026-09-04` ): you decide when older turns are replaced by a summary.
- **Mid-conversation tool changes** (`inline-tools-2026-09-15` ): add or remove tools in a system message instead of editing`tools` .

Both help keep history append-only, which Fable 5.2’s stricter thinking-block check now requires on every account.

## Developer cheat sheet

Every snippet below targets `claude-fable-5-2` and leaves out `thinking`, `temperature`, `top_p` and `top_k`, which Fable 5.2 either ignores or rejects.

## Authentication (cURL)

```
curl https://api.anthropic.com/v1/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-fable-5-2",
    "max_tokens": 16000,
    "output_config": {"effort": "high"},
    "messages": [{"role": "user", "content": "Hello, Claude"}]
  }'
```
## Streaming (required for large outputs)

At `high` and above, set a large `max_tokens`: it caps thinking plus reply together. Stream so long requests don't hit HTTP timeouts.

```
with client.messages.stream(
    model="claude-fable-5-2",
    max_tokens=128000,
    messages=messages,
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
    final = stream.get_final_message()
```
## Structured JSON output

`from pydantic import BaseModel`
class AnalysisResult(BaseModel):

summary: str

key_findings: list[str]

confidence: floatresponse = client.messages.create(

model="claude-fable-5-2",

max_tokens=8192,

output_config={

"format": {"type": "json_schema", "schema": AnalysisResult.model_json_schema()}

},

messages=[{"role": "user", "content": "Analyze this dataset..."}],

)

## Tool use (no forced tools)

`tool_choice` of `any` or a named `tool` returns a 400 on Fable 5.2. Keep `auto` and add `strict: True` for schema-valid inputs.

```
tools = [{
    "name": "search_flights",
    "description": "Search flights to a destination",
    "strict": True,
    "input_schema": {
        "type": "object",
        "properties": {
            "destination": {"type": "string"},
            "departure_date": {"type": "string", "format": "date"},
        },
        "required": ["destination", "departure_date"],
        "additionalProperties": False,
    },
}]
```
```
response = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=4096,
    tools=tools,
    tool_choice={"type": "auto"},
    messages=[{"role": "user", "content": "Find flights to Tokyo next Friday"}],
)
```
To let the SDK run the tool loop for you, use the beta tool runner: `client.beta.messages.tool_runner(...)`.

## Prompt caching

Cache reads cost $0.20 per MTok on Fable 5.2, 2.5% of the base input price.

```
response = client.messages.create(
    model="claude-fable-5-2",
    max_tokens=4096,
    system=[{
        "type": "text",
        "text": "You are an expert code reviewer...",  # large, stable prompt
        "cache_control": {"type": "ephemeral"},
    }],
    messages=[{"role": "user", "content": "Review this PR..."}],
)
```
## Batch API (50% off)

```
batch = client.messages.batches.create(
    requests=[{
        "custom_id": "req-1",
        "params": {
            "model": "claude-fable-5-2",
            "max_tokens": 4096,
            "messages": [{"role": "user", "content": "..."}],
        },
    }]
)
```
```
status = client.messages.batches.retrieve(batch.id)
for result in client.messages.batches.results(batch.id):
    print(result.custom_id, result.result.type)
```
Batch and cache discounts stack. Fast mode and server-side fallback are not available in batches; a refused batch item comes back as `succeeded` with `stop_reason: "refusal"`.

## Quick reference

- **Less thinking, lower cost** →`output_config={"effort": "medium"}` or`"low"`
- **Faster output** →`speed="fast"`
- **Automatic retry on refusal** →`fallbacks="default"` with the`server-side-fallback-2026-07-01` beta
- **Valid JSON** →`output_config.format` or`strict: True` tools
- **Effort change mid-chat** → a system message with`output_config` and the`mid-conversation-output-config-2026-07-01` beta
- **Summarized reasoning** → read the`thinking` blocks; don't ask for reasoning in the reply text

## Benchmarks and what a task really costs

Fable 5.2 ranks first on both independent indexes published so far, but the token price cut does not make every task cheaper: effort decides the bill.

[embed: node/133884d3-d107]

Fable 5.2 at `high` outscores both Fable 5.1 at max and Opus 5.5 at max while costing less per task than either. At max, Fable 5.2 used about 1.7x the output tokens of Fable 5.1, so its cost per task roughly doubled despite the 20% price cut (Atomic Agent).

## Anthropic’s reported benchmarks

Scores shown as Fable 5.2 / Fable 5.1 / Opus 5.5:

- **FrontierCode 2.0:** 49.1% (at xhigh) / 32.4% / 45.5%
- **FrontierSWE v2:** 69.9% / 56.3% / 62.3%
- **AutomationBench:** 51.4% / 31.4% / 42.5%
- **Terminal-Bench-Science 0.1:** 74.6% / 52.6% / 58.7%
- **Remote Labor Index:** 41.3% (at high) / 17.9% / 21.3%
- **Humanity’s Last Exam, with tools:** 74.2% / 65.6% / 67.7%
- **Chartography, no tools:** 73.4% / 44.8% / 64.4%

These are Anthropic’s own numbers at each model’s highest effort unless noted. Anthropic also reports that 1,000 Fable 5.2 agents formalized Fermat’s Last Theorem in about 30 hours.

## Independent results (Vals, high effort)

- **Vals Index:** 71.31%, ranked 1 of 36
- **Vibe Code Bench v1.1:** 93.77%, ranked 1 of 127
- **Finance Agent v2.1:** 69.47%, ranked 1 of 39
- **Terminal-Bench 4.0:** 65.15%, ranked 1 of 63 (tied with Opus 5.5)
- **SWE-bench Multimodal:** 62.29%, ranked 2 of 22
- **Vals Legal Index:** 69.17%, ranked 20 of 37
- **Tax Agent Bench:** 32.09%, ranked 27 of 84

The Vals lead is narrow: Sonnet 5.5 sits under half a point behind. A first place overall does not mean first place in your field, so test on your own workload.

## When to use which model

Start on Opus 5.5 and move to Fable 5.2 when Opus at higher effort still misses your bar; that is Anthropic’s own recommendation (what’s new).

- **Hardest reasoning, research loops, multi-hour agents** → Fable 5.2 at`high` (try`xhigh` only after measuring a gain)
- **Routine agent work where Fable’s ceiling matters** → Fable 5.2 at`medium`
- **Verified security or life-sciences research** → Mythos 5.2 at`high`
- **Most coding and knowledge work** → Opus 5.5 at`medium` , its default ($4 / $20)
- **Speed and intelligence balance at high volume** → Sonnet 5.5,`medium` for agents and`high` otherwise ($2 / $10)
- **Classification, extraction, routing, subagents** → Haiku 5.5 at`low` or`medium` (from $0.10 / $0.50)
- **Zero data retention required** → Opus 5.5 or below, since Fable 5.2 needs 30-day retention

Fable 5.2 also has known soft spots: Vals ranks it mid-table on legal and tax tasks, and Anthropic says it deliberately capped some AI-research skills (such as accelerator kernel optimization) near Opus 5.5 level (Atomic Agent).

## Gotchas and migration tips

Four changes from Fable 5.1 can break working code; the rest are behavior shifts you will notice in prompts (migration guide).

## Breaking changes

1. **Editing history now fails on every account.** If you change`system` ,`tools` or any earlier message before a thinking block, the request returns a 400 (`The block is bound to a different conversation` ). Keep history append-only, or as a stopgap send the`thinking-binding-controls-2026-08-01` beta with`thinking.block_binding.prefix_mismatch_behavior: "drop_block"` .
2. **Fable 5.2 thinking blocks only work on Fable 5.2 and Mythos 5.2.** Routing a conversation to Opus 5.5, Fable 5.1 or a fallback model drops that reasoning. The request still succeeds and dropped blocks aren’t billed.
3. **Computer use moves to** **computer_toolset_20260801**
**.** The old`computer_20251124` tool returns a 400 everywhere except Amazon Bedrock.
4. **The advisor tool needs a Fable 5.2 or Mythos 5.2 advisor** when Fable 5.2 is the executor.

## Rules that return a 400

- `thinking: {"type": "enabled"}` with`budget_tokens` , or`{"type": "disabled"}` . Omit`thinking` or send`{"type": "adaptive"}` .
- `tool_choice` of`any` or a named`tool` .
- Non-default `temperature` ,`top_p` or`top_k` . Just leave them out.

## Behavior to plan for

- **“Don’t think” prompts no longer work** at`medium` and above. Lower effort instead.
- **Long tasks can stop early** with a list of what remains, more often at`low` and`medium` . Say explicitly that the whole task should be finished.
- **Subagents add cost.** At`xhigh` and`max` the model may spawn reviewers of its own work; budget for it or tell it when not to delegate.
- **Don’t ask for reasoning in the reply text.** A`<thinking>` section or a`reasoning` JSON field can trigger a`reasoning_extraction` refusal, which is billed and has no fallback. Read summarized`thinking` blocks instead.
- **Refusals look like successes.** They are HTTP 200, so error-rate monitoring misses them. Log`stop_reason == "refusal"` and fallback-served responses as separate metrics.
- **30-day retention is mandatory.** Fable 5.2 is a Covered Model and isn’t available under zero data retention without Anthropic’s authorization.

## Migrate in one line, then test

```
model = "claude-fable-5-1"  # Before
model = "claude-fable-5-2"  # After
```
Then re-run your evals with an effort sweep starting at `high`, rather than carrying over Fable 5.1 settings.

## Sources

- Claude Fable 5.2 model overview, Claude Platform Docs
- What’s new in Claude Fable 5.2, Claude Platform Docs
- Effort, Claude Platform Docs
- Fast mode, Claude Platform Docs
- Refusals and fallback, Claude Platform Docs
- Claude Fable 5.2: price, benchmarks and effort levels, Atomic Agent, with Artificial Analysis and Vals figures
- Anthropic launches Claude Fable 5.2 and Mythos 5.2, MacRumors
- Anthropic releases Claude Fable 5.2 and Claude Mythos 5.2, CNBC
