---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flashx-price-context-window-specs
title: "GLM 5.3 FlashX: price, context window & specs"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: ["2026-09-18"]
keywords: ["flashx", "model", "tokens", "openrouter", "through", "price", "openai", "million"]
source: https://zolamir.com/en/models/z-ai-glm-5-3-flashx/
source_anchor: ""
source_lines: [1, 80]
section: "GLM_Zhipu"
---
Price: Z.AI's own standard price.

Prices: OpenRouter, checked

## About this model

GLM 5.3 FlashX is an AI model from Z.ai (Zhipu), made available through the API on September 18, 2026. It can read up to 1,048,576 tokens in one request, roughly 1,966 pages of text, and write up to 131,072 tokens in a single answer. Besides text, it accepts images and video as input.

Through the API it costs $0.37 per million input tokens and $1.25 per million output tokens. Its pricing sits in the middle of the market.

It supports extended reasoning: it can "think" step by step before answering, which helps on maths, code and complex questions. It can call tools and functions, so developers can connect it to search, databases or other software.

Not rated yet. Independent test results usually appear a few weeks after a model is released.

Scores: Epoch AI, updated

## Where to use it

Companies that serve this model through an API: 1. Prices per million tokens, cheapest first.

Availability and prices come from OpenRouterâs public API and are refreshed daily. Uptime is the share of successful requests over the last 24 hours. Some providers run a compressed version of open models (fp8, int4), which is cheaper but can be slightly less accurate.

## Use it in your code

Call GLM 5.3 FlashX through OpenRouterâs OpenAI-compatible API: create a key on openrouter.ai, then run one of these examples.

```
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "z-ai/glm-5.3-flashx", "messages": [{"role": "user", "content": "Hello! What can you do?"}]}'
```
```
import os
from openai import OpenAI
client = OpenAI(base_url="https://openrouter.ai/api/v1", api_key=os.environ["OPENROUTER_API_KEY"])
reply = client.chat.completions.create(
    model="z-ai/glm-5.3-flashx",
    messages=[{"role": "user", "content": "Hello! What can you do?"}],
)
print(reply.choices[0].message.content)
```
```
import OpenAI from "openai";
const client = new OpenAI({ baseURL: "https://openrouter.ai/api/v1", apiKey: process.env.OPENROUTER_API_KEY });
const reply = await client.chat.completions.create({
  model: "z-ai/glm-5.3-flashx",
  messages: [{ role: "user", content: "Hello! What can you do?" }],
});
console.log(reply.choices[0].message.content);
```
The same code works with every model on this site: only the model ID changes. Many labs also offer their own API and SDK.

## What it costs in practice

- $0.56**1,000 chat replies**
- $0.02**Summarise a 100-page document**
- $261**An app with 10,000 requests a day for a month**
- $0.015**Translate a 10-page document into Arabic**

## Similar-priced alternatives

## Frequently asked questions

- How much does GLM 5.3 FlashX cost?
- GLM 5.3 FlashX costs $0.37 per million input tokens and $1.25 per million output tokens through the API. For example, 1,000 chat replies of about 500 tokens in and 300 out cost around $0.56.
- What is the context window of GLM 5.3 FlashX?
- GLM 5.3 FlashX can read up to 1,048,576 tokens in one request, about 1,966 pages of text. It can write up to 131,072 tokens in a single answer.
- Is GLM 5.3 FlashX open source?
- No. GLM 5.3 FlashX is a closed model: its weights are not published, so you use it through Z.ai (Zhipu)âs own apps and API, or through cloud providers.
- Who makes GLM 5.3 FlashX?
- GLM 5.3 FlashX is made by Z.ai (Zhipu). It became available through the API on September 18, 2026.
- Can GLM 5.3 FlashX read images?
- Yes. Besides text, it accepts images and video as input.
- What are the alternatives to GLM 5.3 FlashX?
- Models with a similar price from other companies include Beam 1.0 501B A23B, Hy4 and MiMo-V2.6-Pro. You can compare them side by side on this page.

## The week in AI, by email

Every Monday: the new models, the price changes and the stories that mattered. Free, one email a week, unsubscribe in one click.
