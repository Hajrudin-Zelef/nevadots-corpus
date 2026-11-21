---
id: collect-261121-veille-ia/modeles-open/kling-image-4-0-api-pricing-per-image-up-to-8k-yir
title: "Kling Image 4.0 API Pricing per image: Up to 8K | Yir"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["image", "result", "channel", "model", "input", "kling", "openai", "pricing"]
source: https://yir.ai/models/kling-image-4
source_anchor: ""
source_lines: [1, 98]
section: "Modeles_Open"
---
### Automatic routing and failover

Call Kling Image 4.0 with one model ID and Yir picks the channel for each job from FAL and KIE: it goes to the eligible channel with the lowest estimated price and is billed at that channel's public price, with no markup. If an attempt is confirmed failed, Yir retries on another channel within your max_cost. No routing parameters needed in your request. Routing and fallback docs

## Pricing notes

Billed by the channel that actually runs the job. Failover may switch channels, which can change cost and time.

## Use the API

Hand the code to a coding agent, generate right in the chat with MCP or the Skill, or integrate yourself with the SDK or the OpenAI-compatible API.

### Let a coding agent integrate it

Copy the integration prompt, or open it directly in Codex or Claude. The agent reads this page and the model contract, then writes submit, wait and result handling into your project.

### Add the Yir MCP server

Connect it to ChatGPT, Claude or Cursor, then mention @Yir and ask for Kling Image 4.0 by name. It quotes first and only generates after you confirm.

`https://gateway.yir.ai/mcp`
### One command for Claude Code

Create an API key in the console, set it as YIR_API_KEY, then run:

`claude mcp add --transport http yir https://gateway.yir.ai/mcp --header "Authorization: Bearer $YIR_API_KEY"`
### Install the Yir Skill

Set YIR_API_KEY where your agent runs, add the Skill to Claude Code, Codex or another coding agent, then say “use Kling Image 4.0 on Yir to make…”.

`npx skills add yir-ai/skills`
Install the official SDK with your package manager, or copy the code snippet below to get started

| import { createNodeYirClient } from "@yir-ai/sdk/server"; | 
| // Reads YIR_API_KEY from the environment by default: process.env.YIR_API_KEY | 
| const client = createNodeYirClient(); | 
| // 1. Submit an image generation job | 
| const job = await client.submitImage({ | 
| model: "klingai/kling-image-4.0", | 
| input: { | 
| type: "text", | 
| prompt: "Create a citrus sparkling water product poster: a clear glass bottle and fresh oranges, cream background, soft natural light, and a crisp product label", | 
| }, | 
| parameters: { | 
| resolution: "2K", | 
| aspect_ratio: "1:1", | 
| n: 1, | 
| }, | 
| }); | 
| // 2. Wait for the job to finish and deliver its result | 
| const result = await client.waitForJob(job.id); | 
| console.log("Delivered files:", result.result); | 
| if (result.result && "files" in result.result) { | 
| for (const file of result.result.files) { | 
| console.log("->", file.url); | 
| } | 
| } | 

Projects already on the OpenAI protocol, or gateways such as new-api, only need a new Base URL and key. OpenAI compatibility guide

- OpenAI SDK base_url
- `https://gateway.yir.ai/openai/v1`
- new-api channel Base URL
- `https://gateway.yir.ai/openai`
- Key
- Yir API key
- Model ID
- `klingai/kling-image-4.0`

```
curl https://gateway.yir.ai/openai/v1/images/generations \
  -H "Authorization: Bearer $YIR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "klingai/kling-image-4.0",
    "prompt": "Create a citrus sparkling water product poster: a clear glass bottle and fresh oranges, cream background, soft natural light, and a crisp product label",
    "resolution": "2K",
    "aspect_ratio": "1:1"
  }'
```
## Parameter overview

Common parameters; limits depend on the current model and selected channel

- Resolution resolution
  - 2K
  - 4K
  - 8K
 Output resolution tier. Higher resolutions offer sharper pixel fidelity and scale pricing accordingly
- Aspect ratio aspect_ratio
- Geometric aspect ratio. Choose according to target medium (portrait, banner, or square)
- Input mode input_mode
  - Text input
  - Image input
 Reference limits and formats depend on the model and selected channel
- References references
  - reference_image ≤ 10
 Reference limits across input modes depend on the selected channel. Counts in the price table are examples, not input limits or free allowances.
