---
id: collect-261121-veille-ia/gpt-openai/how-to-use-gpt-6-1-terra-openai-s-new-fast-vision-model
title: "How to use GPT-6.1 Terra, OpenAI's new fast vision model"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "terra", "image", "input", "gpt-6", "text", "type", "model"]
source: https://flaviocopes.com/gpt-6-1-terra/
source_anchor: ""
source_lines: [1, 180]
section: "GPT_OpenAI"
---
OpenAI released **GPT-6.1 Terra** on October 26, 2026. It’s the fast, efficient model of the GPT-6 family, built for visual reasoning, computer use and clear writing.

The price is what caught my eye. It costs $1 per million input tokens and $5 per million output tokens. OpenAI says that’s 50% lower than both GPT-5.6 Terra and GPT-6.1 Sol.

Here’s what you get:

- a 1,050,000 token context window (up to 922,000 input tokens)
- up to 128,000 output tokens
- text and image input, text output
- a June 1, 2026 knowledge cutoff

In ChatGPT, Terra is available to Plus, Pro, Enterprise and Edu users in ChatGPT Work and Codex. In this tutorial we’ll use it from the API.

## Your first request

Install the official SDK:

`npm install openai`
Then set your API key:

`export OPENAI_API_KEY=sk-...`
The model ID is `gpt-6.1-terra`. Create a `terra.mjs` file and call it with the Responses API:

```
import OpenAI from 'openai'
const openai = new OpenAI()
const response = await openai.responses.create({
  model: 'gpt-6.1-terra',
  input: 'Write a one-line commit message for a fix to a broken pagination link'
})
console.log(response.output_text)
```
Run it:

`node terra.mjs``Fix broken pagination link`
That took 20 input tokens and 8 output tokens.

## Pick a reasoning effort

Terra supports six reasoning effort values: `none`, `low`, `medium` (the default), `high`, `xhigh` and `max`.

Notice that `none` is there. GPT-6 Astra and GPT-6.1 Sol don’t support it, Terra does. Use it for quick tasks where you want the fastest answer:

```
const response = await openai.responses.create({
  model: 'gpt-6.1-terra',
  reasoning: { effort: 'none' },
  input: 'Translate "Il negozio apre alle 9" to English'
})
```
`“The shop opens at 9.”`
The answer came back in about a second and a half, with no reasoning tokens.

OpenAI’s advice is to start with `medium` and go up only if the better quality is worth the extra time and tokens.

Be careful with sampling parameters. When reasoning effort is not `none`, you need to remove `temperature`, `top_p` and `top_logprobs` from your request.


## Read an image

Vision is where Terra shines. OpenAI suggests using it to match product photos to catalog entries, interpret technical drawings, or read documents like invoices.

I made a sample invoice to test it, from a print shop in Milan:

You pass an image as an `input_image` content item. Here we read a local file and send it as a Base64 data URL:

```
import fs from 'fs'
import OpenAI from 'openai'
const openai = new OpenAI()
const image = fs.readFileSync('/Users/flavio/invoice.png', 'base64')
const response = await openai.responses.create({
  model: 'gpt-6.1-terra',
  input: [
    {
      role: 'user',
      content: [
        { type: 'input_text', text: 'Who sent this invoice, and what is the total?' },
        { type: 'input_image', image_url: `data:image/png;base64,${image}` }
      ]
    }
  ]
})
console.log(response.output_text)
```
`The invoice was sent by **Tipografia Rossi S.r.l.** The total is **€1,220.00**.`
Both are right. The image is 800 by 560 pixels, and the whole request used 558 input tokens.

You can also pass a public image URL in `image_url`, or a `file_id` from the Files API.

## Get structured data out of it

Reading an invoice is nice. Getting JSON you can save to a database is better.

Add a JSON schema in `text.format` and Terra returns data that matches it:

```
const response = await openai.responses.create({
  model: 'gpt-6.1-terra',
  input: [
    {
      role: 'user',
      content: [
        { type: 'input_text', text: 'Extract the invoice details' },
        { type: 'input_image', image_url: `data:image/png;base64,${image}` }
      ]
    }
  ],
  text: {
    format: {
      type: 'json_schema',
      name: 'invoice',
      strict: true,
      schema: {
        type: 'object',
        properties: {
          vendor: { type: 'string' },
          date: { type: 'string' },
          total: { type: 'number' },
          currency: { type: 'string' }
        },
        required: ['vendor', 'date', 'total', 'currency'],
        additionalProperties: false
      }
    }
  }
})
const invoice = JSON.parse(response.output_text)
console.log(invoice.total)
```
This is the `output_text` I got for the sample invoice:

`{"vendor":"Tipografia Rossi S.r.l.","date":"2026-10-20","total":1220,"currency":"EUR"}`
Notice two things. The invoice says “20 October 2026” and “€1,220.00”, and Terra turned them into an ISO date, a number and a currency code without being told to. And `invoice.total` is the number `1220`, ready to store.

The request used 593 input tokens and 71 output tokens, 34 of them for reasoning. That’s less than a tenth of a cent.

This is the kind of high-volume job OpenAI built Terra for: reading incoming invoices and entering them into accounting software.

## Tools need the Responses API

Terra works with Chat Completions too, but tool calling requires the Responses API.

In Responses it supports function calling plus the hosted tools: `web_search`, `file_search`, `code_interpreter`, `computer_use`, `mcp`, `tool_search`, `image_generation`, `hosted_shell`, `apply_patch` and `skills`.

Here’s a web search call:

```
const response = await openai.responses.create({
  model: 'gpt-6.1-terra',
  tools: [{ type: 'web_search' }],
  input: 'What changed in the latest Node.js LTS release?'
})
console.log(response.output_text)
```
## What it costs

Standard pricing, per million tokens:

|  | Price | 
|---|---|
| Input | $1 | 
| Cached input | $0.05 | 
| Cache writes (30 minutes) | $1.25 | 
| Cache writes (12 hours) | $2.50 | 
| Output | $5 | 

A few things change the bill. Prompts over 272K input tokens cost 2x on input and cache and 1.5x on output, for the whole request. Batch and Flex are 50% cheaper. Fast mode costs 2x.

## When to use it

OpenAI says Terra scored 15 percentage points higher than GPT-5.6 Terra on its Agents’ Last Exam computer use benchmark, at medium effort. On Terminal-Bench 4.0 at max effort it solves 50.3% of tasks, up from 21.5%.

For the most complex coding tasks, OpenAI still recommends GPT-6.1 Sol or GPT-6 Astra. My advice is to use Terra for vision, document extraction, computer use and writing, where its speed and price make the biggest difference.

If you’re migrating from GPT-5.6 Terra, run both models on the same images and workflows, at the same reasoning effort, before you switch.

## Sources

Want me to talk about your product? You can sponsor this site.
