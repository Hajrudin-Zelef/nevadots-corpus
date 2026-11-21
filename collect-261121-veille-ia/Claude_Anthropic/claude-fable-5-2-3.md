---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-3
title: "Claude Fable 5.2"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["claude", "fable", "adaptive", "thinking", "data", "tool", "capabilities", "output"]
source: https://platform.claude.com/docs/en/models/fable-5-2/overview
source_anchor: ""
source_lines: [1, 57]
section: "Claude_Anthropic"
---
## Overview

Claude Fable 5.2 is built for demanding reasoning and long-horizon agentic work. Typical uses include agent sessions that run for hours, multistep deep research, and analysis carried through to a finished document, spreadsheet, or deck.

## Claude Fable 5.2 and Claude Mythos 5.2

Claude Mythos 5.2 offers the same capabilities only to organizations verified through Anthropic’s verification programs, such as the Cyber Verification Program. It shares Claude Fable 5.2’s specifications and pricing. To request access, apply to the program that covers your use case, or contact your account team.

## How it compares

| Model | Context | Max output | Price / MTok | Latency | Thinking | Default effort | Knowledge cutoff | 
|---|---|---|---|---|---|---|---|
| Claude Fable 5.2 | 1M | 128K | $8 / $40 | Moderate | Adaptive (always on) | `high` | Jun 2026 | 
| Claude Opus 5.5 | 1M | 128K | $4 / $20 | Moderate | Adaptive (always on) | `medium` | Jun 2026 | 
| Claude Sonnet 5.5 | 1M | 128K | $2 / $10 | Fast | Adaptive | `high` | Jun 2026 | 
| Claude Haiku 5.5 | 1M | 128K | From $0.10 / $0.50 | Fastest | Adaptive | `medium` | Jun 2026 | 

## Specifications

### Model IDs

- Claude API
- `claude-fable-5-2`

### Capabilities

- Max output
- 128K tokens
- Comparative latency
- Moderate
- Input → output
- Text and images → text
- Reliable knowledge cutoff
- Jun 2026
- Training data cutoff
- Jun 2026

### Availability

- Released
- November 9, 2026
- Retirement
- Not sooner than November 9, 2027

## Good to know

- Adaptive thinking is always on and can’t be turned off. Control thinking depth with the effort parameter.
- Forced tool use (`tool_choice` types`any` and`tool` ) returns a 400 error. See Thinking and tool choice.
- Omit `temperature` ,`top_p` , and`top_k` . See Sampling parameters for the values that return a 400 error.
- Claude Fable 5.2 requires 30-day data retention and isn’t available under zero data retention unless expressly authorized by Anthropic. See Model-specific data retention requirements.
- Query limits and capabilities programmatically with the Models APIPreview with links to the API reference available — press Tab until focus enters the card, Escape to return..

## Resources

## Reference

Was this page helpful?
