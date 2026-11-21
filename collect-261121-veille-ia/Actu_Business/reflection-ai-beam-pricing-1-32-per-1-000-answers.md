---
id: collect-261121-veille-ia/actu-business/reflection-ai-beam-pricing-1-32-per-1-000-answers
title: "Reflection AI Beam Pricing: $1.32 per 1,000 Answers"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-10-05"]
keywords: ["beam", "reflection", "model", "openrouter", "host", "hosts", "request", "wordpress"]
source: https://mxchat.ai/reflection-beam-pricing-chatbot-test/
source_anchor: ""
source_lines: [1, 164]
section: "Actu_Business"
---
Reflection AI’s Beam 1.0 is the first open-weight model from the Nvidia-backed lab, and on OpenRouter it costs **$0.25 to $0.40 per million input tokens and $1.00 to $1.20 per million output tokens**, depending on which of its five hosts serves the request. Beam is a sparse mixture-of-experts model with 501 billion parameters in total and 23 billion active per token, and a context window of about 1 million tokens. Reflection announced it on 5 October 2026, published the weights on 16 October, and says it matches Z.ai’s GLM-5.2 on its own benchmarks while using a fraction of the compute.

Benchmarks do not tell a WordPress site owner what a chatbot will cost. So on 1 November 2026 we sent the same support-bot request from our own WordPress server to Beam 1.0 on each of its five hosts, at three reasoning-effort levels, and with reasoning switched off. We ran the same request against four rival models. That came to 96 calls, and every answer that came back was checked against our documentation.

**The short answer:** on Nebius, the cheapest host, Beam 1.0 cost **$1.32 per 1,000 answers** with no cache and answered in a **1.21-second median**. When the prompt repeated, CoreWeave’s cache cut the bill to **$0.40**. The fastest host was Together at **0.80 seconds**. Claude Haiku 5.5 cost $0.87 on the same request and took 2.34 seconds. Every one of Beam’s 56 answers was correct. And, as with several new reasoning models, Beam will not let you switch reasoning off: every attempt returned HTTP 400.

## Beam 1.0 pricing at a glance

These are the rates on OpenRouter’s model API on 1 November 2026. OpenRouter lists one model, `reflection/beam-1.0-501b-a23b`, with five hosts behind it. Every host offers the full 1,048,576-token context window.

| Host | Input / 1M | Cached input / 1M | Output / 1M | Weights | 
|---|---|---|---|---|
| **Nebius** | $0.25 | $0.05 | $1.00 | FP4 | 
| CoreWeave | $0.30 | $0.04 | $1.00 | FP4 | 
| Baseten | $0.40 | $0.04 | $1.20 | FP4 | 
| Together | $0.40 | $0.04 | $1.20 | not stated | 
| DigitalOcean | $0.40 | $0.04 | $1.20 | not stated | 

That puts Beam well below the other large new models we have priced this autumn: **Kimi K3.1 Pro** lists at $2.00 / $10.00, and StepFun’s **Step 5 Preview** at $1.00 / $2.70. It sits above the small, fast models a support bot usually runs on: **Claude Haiku 5.5** lists at $0.10 / $0.50 and **DeepSeek V4.1 Flash** at $0.15 / $0.60.

Reports of the launch say Reflection’s own API is in a free public beta. We did not test it; this post covers the paid hosts on OpenRouter, which is how a WordPress plugin like MxChat reaches the model.

## How we tested it

Every request was the one a MxChat support bot actually sends. It contained the site’s real system prompt plus four passages pulled from our own documentation, about 4,180 tokens on Beam’s count, followed by one visitor question. We asked four questions, each twice:

1. Does MxChat work with WooCommerce, and can the bot look up an order status for a logged-in customer?
2. Do I need Pinecone to use the knowledge base, or can I store embeddings in WordPress?
3. Can a visitor be handed over to a human on Slack or Telegram when the bot cannot answer?
4. Can I use my own OpenRouter or OpenAI API key, and which models can I choose from?

We ran 12 setups:

- **Beam 1.0** on each of its five hosts at the default setting, each host pinned with fallbacks disabled.
- Beam 1.0 on Nebius with reasoning effort set to **low** , set to**high** , and with reasoning**switched off** .
- **Claude Haiku 5.5** , the cheap default we recommend most often.
- **GLM-5.3** , the closest model to the GLM-5.2 that Reflection compares Beam against, plus**DeepSeek V4.1 Flash** and**Nemotron 3.5 Super** , the open model Nvidia itself publishes. For these three we let OpenRouter pick the host, as MxChat does.

All calls went through OpenRouter from our WordPress server in the US. They were non-streamed, with usage accounting switched on, so OpenRouter reported the exact tokens and billed cost of every call. The first pass of each setup gives the “cold” cost. The second, identical pass shows what each host’s caching does. We checked every answer against the facts in the passages. For example, the WooCommerce answer had to say the feature needs MxChat Pro or its add-on and a logged-in customer, and the Pinecone answer had to say WordPress storage works.

## What 1,000 chatbot answers cost

| Setup | Cost per 1,000 answers (cold) | Billed on repeat | Median answer time | Output tokens per answer | Correct | 
|---|---|---|---|---|---|
| **Beam on Nebius** | **$1.32** | $1.01 | 1.21 s | 272 | 8/8 | 
| Beam on Nebius, effort low | $1.27 | $0.95 | 1.10 s | 222 | 8/8 | 
| Beam on Nebius, effort high | $1.30 | $0.63 | 1.25 s | 251 | 8/8 | 
| Beam on Nebius, reasoning off | HTTP 400 on all 8 calls: “Reasoning is mandatory for this endpoint and cannot be disabled.” |  |  |  | 0/8 | 
| Beam on CoreWeave | $1.48 | **$0.40** | 1.05 s | 228 | 8/8 | 
| Beam on Baseten | $1.95 | $1.43 | 0.98 s | 230 | 8/8 | 
| Beam on Together | $1.96 | $0.49 | **0.80 s** | 243 | 8/8 | 
| Beam on DigitalOcean | $1.99 | $0.46 | 1.46 s | 261 | 8/8 | 
| Claude Haiku 5.5 | $0.87 | $0.88 | 2.34 s | 372 | 8/8 | 
| GLM-5.3 (host picked by OpenRouter) | $2.67 billed | $1.44 | 1.69 s | 161 | 8/8 | 
| DeepSeek V4.1 Flash (host picked by OpenRouter) | $0.40 billed | $0.24 | 1.46 s | 262 | 8/8 | 
| Nemotron 3.5 Super (host picked by OpenRouter) | $0.62 billed | $0.52 | 8.75 s | 456 | 8/8 | 

Three things stand out.

**Input sets the price, so the host’s input rate decides the bill.** Our prompt was about 4,180 tokens. At Nebius’s $0.25 per million, input costs $1.05 per 1,000 answers, about four fifths of the $1.32 total. Output adds only about $0.27. That is why the same model costs half as much again on Together, Baseten or DigitalOcean: their $0.40 input rate adds about $0.63 per 1,000 answers before a word is written, and their slightly higher output rate adds a few cents more.

**Most of the output is reasoning, but there is not much of it.** OpenRouter split Beam’s reasoning from its answers, and on Nebius about 200 of the 272 output tokens per answer, roughly 73%, were thinking the visitor never sees. That share is normal for a reasoning model. What is unusual is how little Beam thinks in total: 272 tokens against 495 for StepFun’s Step 5 Preview and 456 for Nemotron 3.5 Super on the same request. Reflection’s claim that Beam uses less inference compute shows up here as a smaller bill.

**The tokenizer helps.** The same request counted as about 4,180 tokens on Beam and 6,838 on Claude, 39% fewer. That is part of why Beam on Nebius is only half as expensive again as Haiku 5.5, even though its input rate is two and a half times Haiku’s.

Every Beam answer was correct and complete. They averaged 39 to 51 words, named the Pro requirement for WooCommerce order lookups, said WordPress can store the embeddings without Pinecone, and listed both Slack and Telegram for live handoff. The four rival models were also correct on every question. Our automatic check flagged one Nemotron answer because it wrote “key” instead of “API key”; we read it, and it was right.

## Reasoning cannot be switched off, and effort barely matters

On many reasoning models you can save money and time by turning thinking off for simple support questions. Beam 1.0 does not allow it. With `"reasoning": {"enabled": false}` on OpenRouter, the Nebius endpoint rejected all eight calls with HTTP 400 and the message “Reasoning is mandatory for this endpoint and cannot be disabled.” It did not fall back to answering without reasoning. StepFun’s Step 5 Preview returned the same error in our test yesterday.

Unlike Step 5, Beam hardly responds to the effort setting either. Low effort wrote 222 output tokens per answer and cost $1.27 per 1,000 answers. The default wrote 272 and cost $1.32. High effort wrote 251 and cost $1.30, which is within the run-to-run noise of the default. On short support questions Beam already thinks briefly, so there is little to trim. Low effort was the fastest Nebius setting at 1.10 seconds and gave the shortest answers, 39 words on average, with no loss of accuracy.

This matters for plugin users. A chatbot plugin that sends `reasoning: {enabled: false}` to every model to keep costs down would get errors from Beam, not cheaper answers. MxChat does not send a reasoning setting on the OpenRouter route, so the model runs at its default effort, and on Beam the default is already efficient.

## Speed: every host is fast enough for live chat

The last two models we tested, StepFun’s Step 5 and Nvidia’s Nemotron 3.5 Super, showed that the same model can be five times slower or worse depending on which host serves it. Beam does not have that problem. Its five hosts ranged from a 0.80-second median on Together to 1.46 seconds on DigitalOcean, and the slowest single Beam answer in the whole test took 3.12 seconds, on CoreWeave.

Together was both the fastest and the most consistent host, with every answer between 0.65 and 1.01 seconds and a median output speed of about 325 tokens per second. Baseten came second at 0.98 seconds. Nebius, the cheapest host, took 1.21 seconds, which is still about half as long as Claude Haiku 5.5’s 2.34 seconds on the same request.

Of the rival models, GLM-5.3 took 1.69 seconds and DeepSeek V4.1 Flash 1.46 seconds on hosts OpenRouter chose. Nemotron 3.5 Super was the outlier again: its median was 8.75 seconds and its slowest answer 27.75 seconds, because OpenRouter routed some calls to slower hosts. A visitor will wait two seconds in a chat window. They will not wait twenty-eight.

## Prompt caching: pick the host that caches the whole prompt

A chatbot sends nearly the same system prompt and knowledge passages again and again, and every Beam host charges a fifth or less of its input rate for cached tokens. On the second, identical pass, the hosts split into two groups:

- **CoreWeave, Together and DigitalOcean** cached essentially the whole prompt, about 4,160 of 4,180 tokens. Their bills fell by 73% to 77%, to**$0.40** , $0.49 and $0.46 per 1,000 answers.
- **Nebius and Baseten** cached only about 1,470 tokens, roughly the shared system prompt. Nebius fell from $1.32 to $1.01 and Baseten from $1.95 to $1.43.
- **Claude Haiku 5.5** cached nothing, because Anthropic needs explicit cache breakpoints, which these calls did not send.

Caching reverses the host ranking. Cold, Nebius is the cheapest Beam host. Warm, it is the second most expensive, and CoreWeave at $0.40 costs less than half of what Haiku 5.5 does. On a site where the same knowledge passages come up again and again, CoreWeave is the host to prefer. Note that caching is per host, so it only pays off if your requests keep landing on the same one.

## What it costs per month

Here is what the measured costs come to at three traffic levels with no cache savings, and at 10,000 answers with a warm cache in the last row:

| Answers per month | Beam on Nebius | Beam on CoreWeave | Beam on Together | Claude Haiku 5.5 | GLM-5.3 | DeepSeek V4.1 Flash | 
|---|---|---|---|---|---|---|
| 1,000 | $1.32 | $1.48 | $1.96 | $0.87 | $2.67 | $0.40 | 
| 10,000 | $13.18 | $14.83 | $19.64 | $8.70 | $26.66 | $3.96 | 
| 100,000 | $132 | $148 | $196 | $87 | $267 | $40 | 
| 10,000, warm cache | $10.13 | $4.02 | $4.85 | $8.76 | $14.42 | $2.37 | 

For a small WooCommerce store answering 10,000 questions a month, Beam comes to $4 to $20 depending on the host and how well the prompt caches. That is cheaper than GLM-5.3 in every row, and on a cache-friendly host it is cheaper than Claude Haiku 5.5 too. DeepSeek V4.1 Flash is still the cheapest model in the test.

## Where Beam 1.0 falls short for a chatbot

- **It is built for coding and agents.** Reflection describes Beam as a model for coding and long-horizon agentic work. A knowledge-base chatbot answering short questions uses little of that, and the 1-million-token context is far more than a support prompt needs.
- **Reasoning is mandatory.** You cannot turn it off. Effort settings change little, so you pay for a small amount of thinking on every answer.
- **Host prices differ by half.** The same model costs $1.32 or $1.99 per 1,000 answers depending on the host, and the cheapest cold host is not the cheapest warm one.
- **It is brand new.** The weights went public on 16 October. Reflection’s benchmark claims are its own, and hosts and prices for a new open model often change in the first months. Test it again before you rely on it.
- **Cheaper models answered just as well.** On our four questions, Haiku 5.5 and DeepSeek V4.1 Flash were also correct every time. Beam’s extra capacity does not show up on a simple support bot.

## How to use Beam 1.0 in a WordPress chatbot

MxChat reaches Beam 1.0 through OpenRouter:

1. Create an OpenRouter API key and add it under **MxChat → Settings → API Keys** .
2. In the chatbot’s model setting, choose `reflection/beam-1.0-501b-a23b` from the OpenRouter model list. The list is read live from OpenRouter, so new models appear without a plugin update.
3. Ask your bot the five questions your visitors ask most, check the answers against your knowledge base, and time them before you switch it on for everyone.

MxChat does not pin a host, so OpenRouter chooses one, usually by price. If your bot repeats the same knowledge passages, you can set a provider preference in your OpenRouter account to favour CoreWeave, which cached the whole prompt in our test. Our OpenRouter pricing test covers its fees against going direct. The MxChat documentation covers knowledge-base setup, and MxChat Pro adds the WooCommerce and live-agent features our test questions asked about.

## Beam against the other models we have tested

We have run the same chatbot request against most of this year’s models:

- GLM-5.3: $2.67 per 1,000 answers today and $2.16 yesterday on hosts OpenRouter chose, so the router’s choice moves the price. Beam on Nebius is about half the price and faster.
- StepFun Step 5 Preview: $5.50 per 1,000 answers and 2.95 seconds in our 31 October test. Beam is a quarter of the price and more than twice as fast.
- Kimi K3.1 Pro: $12.76 per 1,000 answers and a 16-second median in our 29 October test.
- Nemotron 3.5 Super: $0.62 today, cheaper than Beam, but 8.75 seconds on hosts OpenRouter chose.
- DeepSeek V4.1 Flash: $0.40 and 1.46 seconds today, the cheapest model in this test.
- Claude Haiku 5.5: $0.87 and 2.34 seconds, the safest cheap default.

Beam 1.0 is the best value of the large open models we have tested this autumn. It gave correct, complete answers on every call, answered in about a second on every host, and costs less than GLM-5.3, Step 5 or Kimi K3.1 Pro. For a plain knowledge-base bot, Haiku 5.5 or DeepSeek V4.1 Flash still cost less cold. But if your bot also runs agent-style tasks and you want an open model, Beam on CoreWeave or Together is the one to try first.

## FAQ

### How much does Reflection AI’s Beam 1.0 cost?

On OpenRouter on 1 November 2026, Beam 1.0 cost $0.25 per million input tokens and $1.00 per million output tokens on Nebius, $0.30 / $1.00 on CoreWeave, and $0.40 / $1.20 on Baseten, Together and DigitalOcean. On our chatbot request that came to $1.27 to $1.99 per 1,000 answers with no cache, and $0.40 to $1.43 when the prompt was cached.

### Is Beam 1.0 open source?

The weights are published on Hugging Face, and reports of the launch describe an Apache 2.0 licence. Check the licence on the model card before you self-host or ship it in a product.

### Can you turn off reasoning on Beam 1.0?

No. The Nebius endpoint returned HTTP 400 with “Reasoning is mandatory for this endpoint and cannot be disabled.” You can set reasoning effort to low, which cost 4% less and answered slightly faster in our test.

### Which host is best for Beam 1.0?

For the lowest cold price, Nebius at $1.32 per 1,000 answers. For a chatbot that repeats its prompt, CoreWeave, which cached the whole prompt and fell to $0.40. For speed, Together, at a 0.80-second median. Every host answered in under 1.5 seconds at the median.

### Is Beam 1.0 cheaper than GLM-5.3?

Yes. On our request GLM-5.3 cost $2.67 per 1,000 answers on hosts OpenRouter chose, against $1.32 for Beam on Nebius, and it was slower at 1.69 seconds against 1.21.

### Does Beam 1.0 work with MxChat?

Yes, through OpenRouter. Add an OpenRouter key and pick `reflection/beam-1.0-501b-a23b` from the model list.

*Method note: 96 chat-completion calls through OpenRouter from mxchat.ai’s WordPress server on 1 November 2026, 12 setups × 4 questions × 2 runs. 88 returned an answer; 8 were HTTP 400 (reasoning off is not allowed). Beam hosts pinned with fallbacks disabled; other models on OpenRouter’s default routing. Costs are measured token counts at list price with no cache, except GLM-5.3, DeepSeek V4.1 Flash and Nemotron 3.5 Super, which are first-pass billed costs because their hosts varied. Repeat-pass costs are what OpenRouter billed. Times are the median wall-clock time to a complete, non-streamed answer. Prices, quantizations and context windows read from OpenRouter’s model API on 1 November 2026. Release dates and benchmark claims are Reflection’s, as reported at launch.*
