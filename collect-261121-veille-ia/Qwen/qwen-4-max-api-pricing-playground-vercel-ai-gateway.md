---
id: collect-261121-veille-ia/qwen/qwen-4-max-api-pricing-playground-vercel-ai-gateway
title: "Qwen 4 Max API, Pricing & Playground | Vercel AI Gateway"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["gateway", "qwen", "alibaba", "generatetext", "model", "options", "import", "main"]
source: https://vercel.com/ai-gateway/models/alibaba/qwen4-max
source_anchor: ""
source_lines: [1, 107]
section: "Qwen"
---
Qwen 4 Max brings deeper knowledge, richer world understanding, and stronger coding and agentic capabilities. It is built for real-world tasks, from complex coding and knowledge work to front-end builds and professional software workflows. Your use is subject to Alibaba Cloud's Terms & Privacy Policies.

View API reference
- Input and output price
- Input $1.50, Output $4.50, Per 1M tokens
- 24h uptime
- Loading AI Gateway uptime

Qwen 4 Max

## Getting started

Call Qwen 4 Max through AI Gateway with the AI SDK `generateText` and `streamText` functions, or through the OpenAI Chat Completions, OpenAI Responses, and Anthropic Messages APIs by changing the base URL. AI Gateway authenticates the request and routes it to an available provider.

Install the AI SDK (`pnpm add ai dotenv`), create an API key from the API Keys page, and set it as `AI_GATEWAY_API_KEY` in your environment. Full setup is covered in the text generation quickstart.

```
import { generateText } from 'ai';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    prompt: 'Why is the sky blue?',  });
  console.log(result.text);}
main().catch(console.error);
```
## Top-level parameters

The same Qwen 4 Max request in each API format AI Gateway supports.

```
import { generateText } from 'ai';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    system: 'You are a concise technical assistant.',    prompt: 'Summarize the tradeoffs between static generation and SSR.',    maxOutputTokens: 1024,  });
  console.log(result.text);}
main().catch(console.error);
```
Standard parameters like `prompt`, `messages`, `temperature`, and `tools` work as documented in the AI SDK docs. These are the parameters with model-specific behavior.

| Parameter | Type | Required | Description | 
|---|---|---|---|
| `model` | `string` | Yes | Model ID in the form `creator/model` , e.g.`alibaba/qwen4-max` . AI Gateway routes the request to an available provider. | 
| `maxOutputTokens` | `number` | No | Hard cap on generated tokens. Qwen 4 Max supports up to `384,000` output tokens. Reasoning tokens count toward this limit. | 
| `reasoning` | `'provider-default' \| 'none' \| 'minimal' \| 'low' \| 'medium' \| 'high' \| 'xhigh'` | No | Provider-agnostic reasoning effort, available in AI SDK 7 or later. Maps to the provider’s native reasoning configuration; reasoning settings under `providerOptions` take precedence when both are set. See the Reasoning section below. | 
| `providerOptions` | `Record<string, JSONValue>` | No | AI Gateway routing options under `gateway` , plus any provider-native options under the provider’s own namespace — see the table below. | 

## Input limits

| Input | Formats | Sources | Max count | Max size | Limits | 
|---|---|---|---|---|---|
| Text | — | — | — | — | Prompt and response share the 1M-token context window | 
| Image | — | URL, base64, Uint8Array | — | — | Sent as image parts in messages; counts as input tokens | 

## Provider options

Set AI Gateway routing options under `providerOptions.gateway`. For provider-specific options, pass them under the provider’s namespace as documented by the AI SDK.

Learn more in the AI SDK alibaba provider docs.

```
import { generateText } from 'ai';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    prompt: 'Why is the sky blue?',    providerOptions: {      gateway: {        only: ['alibaba'],      },    },  });
  console.log(result.text);}
main().catch(console.error);
```
These AI Gateway routing options apply to every model. Provider-specific options pass through under the provider’s own namespace (for example `providerOptions.anthropic`) exactly as documented by the AI SDK.

| Parameter | Type | Required | Description | 
|---|---|---|---|
| `providerOptions.gateway.only` | `string[]` | No | Restrict routing to these provider slugs. Requests fail over only within the listed providers. | 
| `providerOptions.gateway.order` | `string[]` | No | Preferred provider order. Listed providers are tried first; unlisted providers remain available as fallbacks. | 
| `providerOptions.gateway.sort` | `'cost' \| 'ttft' \| 'tps'` | No | Rank candidate providers by price, time to first token, or tokens per second instead of the default routing order. | 
| `providerOptions.gateway.zeroDataRetention` | `boolean` | No | Route only to providers with a zero-data-retention policy for this model. | 

### Routing across providers

AI Gateway serves the same model through multiple providers and fails over automatically. `order` expresses a preference while keeping every provider eligible; `only` is a hard allowlist — if none of the listed providers are available the request fails instead of falling back.

Options under a provider's own namespace (for example `providerOptions.anthropic`) are forwarded to that provider with the request. Providers ignore option namespaces that don't apply to them, so it is safe to set provider options alongside `gateway` routing options.

## Reasoning

AI Gateway bridges reasoning across every API format. The AI SDK exposes a provider-agnostic top-level `reasoning` level (`none`, `minimal`, `low`, `medium`, `high`, or `xhigh`); the Chat Completions and Responses formats take the same effort under `reasoning.effort`; and the Anthropic Messages format uses a native `thinking` token budget. Whichever you send, the gateway maps it to the target model’s native configuration, converting between effort levels and token budgets as needed. Reasoning-related settings under `providerOptions` take full precedence over the top-level `reasoning` value and are never merged. Reasoning tokens typically count toward your output-token usage, though how they’re reported and billed varies by provider.

Learn more in the AI Gateway reasoning guide.

```
import { generateText } from 'ai';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    prompt: 'Explain the Monty Hall problem step by step.',    reasoning: 'high',  });
  console.log(result.text);}
main().catch(console.error);
```
## Image input

Send images alongside text as message parts. Images count as input tokens.

```
import { generateText } from 'ai';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    messages: [      {        role: 'user',        content: [          { type: 'text', text: 'Describe this image.' },          { type: 'image', image: 'https://example.com/photo.jpg' },        ],      },    ],  });
  console.log(result.text);}
main().catch(console.error);
```
## Tool calling

Expose tools the model can call. Define each tool’s inputs with a Zod schema.

```
import { generateText, tool } from 'ai';import { z } from 'zod';import 'dotenv/config';
async function main() {  const result = await generateText({    model: 'alibaba/qwen4-max',    prompt: 'What is the weather in San Francisco?',    tools: {      getWeather: tool({        description: 'Get the current weather for a location',        inputSchema: z.object({ location: z.string() }),        execute: async ({ location }) => ({ location, temperatureC: 18 }),      }),    },  });
  console.log(result.text);}
main().catch(console.error);
```
