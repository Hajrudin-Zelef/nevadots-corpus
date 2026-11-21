---
id: collect-261121-veille-ia/grok-xai/grok-4-7-cursor-docs-2
title: "Grok 4.7 | Cursor Docs"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "cursor", "work", "more", "standard", "input", "tokens", "effort"]
source: https://prod.cursor.com/docs/models/grok-4-7
source_anchor: ""
source_lines: [1, 33]
section: "Grok_xAI"
---
# Grok 4.7

Grok 4.7 is a frontier model from Cursor and SpaceXAI for coding and knowledge work. It builds on Grok 4.6 with a larger base, longer training on harder multi-hour tasks, and stronger self-verification.

- Stays with difficult, long-running work and checks its own results more carefully than Grok 4.6.
- Manages longer context more reliably, with a 256k standard window and 500k long context.
- Stronger on complex coding and multi-hour knowledge work.

Grok 4.7 supports four effort levels: xhigh, high (default), medium, and low. Higher effort gives the model more time to work through difficult tasks. Effort levels are more separated than in Grok 4.6, so harder tasks spend more time thinking.

Fast is the default speed tier on Premium and higher, and on the legacy Pro, Pro+, and Ultra plans. On Plus and on the legacy Start plan (India only), Grok 4.7 is fixed at medium effort at standard speed.

Grok 4.7 has access to all agent tools when used with Cursor, including:

Find files by name, read directory structures, and grep for patterns.

Generate search queries and fetch results from the web.

Read file contents, including images for vision-capable models.

Suggest edits and apply them automatically.

Execute terminal commands and monitor output.

Generate images from text descriptions or reference images.

Ask clarifying questions while continuing to work in the background.

Learn more about how tools work and tool calling fundamentals.

Grok 4.7 draws from the weekly pool of Cursor models included with every paid plan, alongside Grok 4.6, Grok 4.5, and Composer 2.5. On legacy Cursor plans, it draws from the monthly Cursor Models pool.

Additional usage beyond the pool at standard speed is priced at $2/M input tokens, $0.50/M cached input tokens, and $6/M output tokens. Fast is $4/M input, $1/M cached, and $12/M output. When input exceeds 256k tokens, standard requests bill at 2x those rates and Fast requests bill at 3x the standard rates, up to a 500k context window. Long-context billing uses the input length only: if input is at or below 256k tokens, the full request uses the standard or Fast rates. All prices are per million tokens.
