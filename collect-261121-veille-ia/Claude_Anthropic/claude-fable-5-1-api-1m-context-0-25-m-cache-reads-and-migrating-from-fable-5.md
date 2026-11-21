---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-api-1m-context-0-25-m-cache-reads-and-migrating-from-fable-5
title: "Claude Fable 5.1 API: 1M Context, $0.25/M Cache Reads and Migrating from Fable 5"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "claude", "model", "cache", "anthropic", "output", "thinking", "messages"]
source: http://dev.to/hopbase/claude-fable-51-api-1m-context-025m-cache-reads-and-migrating-from-fable-5-318g
source_anchor: ""
source_lines: [1, 43]
section: "Claude_Anthropic"
---
**The short answer: Claude Fable 5.1 (model ID `claude-fable-5-1`) is live on HopBase; check `GET /v1/models` for your key, and existing keys only need the model name changed. It is called through the native Anthropic Messages API at `/v1/messages`, so the Anthropic SDK and Claude Code work with zero code changes. It has a 1M context window and 128K max output, and adaptive thinking is always on: you steer reasoning depth with `output_config.effort` instead of a `thinking` parameter. The list price matches Fable 5 at $10 input and $50 output per million tokens, with cache writes at $12.50 (5 min) or $20 (1 hour), but cache reads drop to $0.25 from $1.00, so long sessions and agent loops that re-read the same prefix save the most. When migrating, note three changes: `tool_choice` `any` or `tool` returns 400, thinking blocks must be passed back unchanged with append-only history, and assistant prefill is not supported.**

Anthropic's newest flagship, **Claude Fable 5.1** (model ID `claude-fable-5-1`), is live on HopBase. Check `GET /v1/models` for your key — existing keys only need the model name changed.

- 
**1M context window, 128K max output** : whole repositories, long documents, and long multi-turn conversations fit in a single request, with no need to split long outputs.
- 
**Adaptive thinking, always on** : no`thinking` parameter needed; steer reasoning depth and spend with`output_config.effort` (low through max).
- 
**Built for long-horizon agent work** : stronger than Fable 5 at long-running autonomous coding, document / spreadsheet / slide work, multi-step research, vision, and computer use. Same tokenizer as Fable 5, so token counts are essentially unchanged.
- 
**Native Anthropic Messages API** : standard`/v1/messages` ; the Anthropic SDK and Claude Code switch over with zero code changes.

| Item (per million tokens) | Claude Fable 5.1 | Claude Fable 5 | 
|---|---|---|
| Input | **$10** | $10 | 
| Output | **$50** | $50 | 
| Cache write (5 min / 1 hour) | **$12.50 / $20** | $12.50 / $20 | 
| Cache read (hit) | **$0.25** (0.025×) | $1.00 | 

Cache reads cost a quarter of Fable 5's, so long sessions and agent loops that re-read the same prefix save the most. Billing is list price × actual usage, visible in the console in real time; for enterprise volume, request a quote from the pricing page.

- 
**Forced tool use is gone** :`tool_choice``any` /`tool` returns 400. Use`auto` plus an instruction naming the tool, and`strict: true` or structured outputs when you need schema-valid JSON.
- 
**Thinking blocks are bound to the model** : pass them back unchanged within a conversation and keep the history append-only — editing earlier turns invalidates earlier thinking blocks.
- 
**No assistant prefill** : as with every Claude model since 4.6, control output format with structured outputs or the system prompt.

```
curl https://api.hop-base.com/v1/messages \
  -H "x-api-key: sk-your-key" \
  -H "anthropic-version: 2023-06-01" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "claude-fable-5-1",
    "max_tokens": 1024,
    "messages": [{"role": "user", "content": "Hello, introduce yourself"}]
  }'
```
Full setup steps for Claude Code and the Anthropic SDK are in the Anthropic protocol guide. If you already have a key, just switch the model name to `claude-fable-5-1`.

## Top comments (0)
