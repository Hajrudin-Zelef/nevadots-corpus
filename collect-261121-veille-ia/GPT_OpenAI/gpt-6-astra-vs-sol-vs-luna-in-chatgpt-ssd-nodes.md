---
id: collect-261121-veille-ia/gpt-openai/gpt-6-astra-vs-sol-vs-luna-in-chatgpt-ssd-nodes
title: "GPT-6 Astra vs Sol vs Luna in ChatGPT · SSD Nodes"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-07"]
keywords: ["gpt-6", "astra", "luna", "model", "chatgpt", "openai", "page", "gpt-5"]
source: https://www.ssdnodes.com/learn/gpt-6-astra-vs-sol-vs-luna-in-chatgpt
source_anchor: ""
source_lines: [1, 236]
section: "GPT_OpenAI"
---
## The short answer

GPT-6 Astra vs Sol vs Luna is a question about tiers. Astra is the top model in the GPT-6 family. Sol is the middle tier. Luna is the low-cost tier. OpenAI's latest-model guide now recommends GPT-6.2 Astra for the most demanding tasks and GPT-6.2 Sol for complex work at lower cost. Its GPT-6 and GPT-6.1 section still covers GPT-6 Astra, GPT-6.1 Sol, GPT-6.1 Terra and GPT-6 Luna. The Astra model page now describes GPT-6 Astra as a reasoning model for complex coding and professional work, and the Luna page calls it "Our most efficient model for focused, high-volume tasks." In ChatGPT, Terra is an option in Work and Codex, not in the chat window.

GPT-6 is now in ChatGPT, but not on every plan. OpenAI's deployment safety page for the October release, published October 7, 2026, announced: "We're launching GPT-6 to everyone in ChatGPT, across free and paid plans globally." That is not where things stand today. OpenAI's ChatGPT model help page, checked on November 19, 2026, says GPT-6 is rolling out as the current default in Chat for eligible Pro, Plus, Enterprise and Business users. For Free and Go, it says: "GPT-5.6 Luna is becoming the default model for Free and Go users." The safety page also said the new Sol and Luna "will replace GPT-5.6 Sol and GPT-5.6 Luna in ChatGPT", but on Free and Go, GPT-5.6 Luna is still the model.

This guide is for people who use ChatGPT in the chat window. It explains what each tier is for and which plan gets which model. It also gives the price of the same models in the API (application programming interface, the paid developer service). Where OpenAI has not confirmed something on a page we could open, the guide says so.

## Why older guides call Sol the best model

The name "Sol" has belonged to more than one model. Before this release, ChatGPT ran GPT-5.6 Sol and GPT-5.6 Luna. OpenAI's safety page for Astra calls it "the most capable model we have ever broadly deployed", and it compares Astra mainly against GPT-5.6 Sol. So a guide written for the GPT-5.6 lineup that calls Sol the best OpenAI model is describing a lineup that is now being replaced on paid plans. In GPT-6, Astra sits above Sol.

The second trap is timing. Some guides from late September 2026 say Sol and Luna are not in ChatGPT yet. GPT-6 has been rolling out in Chat to paid plans since October 7, so those guides are out of date for Plus, Pro, Business and Enterprise. Guides that say every plan already has GPT-6 are wrong in the other direction: OpenAI's model help page lists no GPT-6 model for Free or Go in Chat. Check the date on any guide before you trust its ranking.

The third trap is the version number. GPT-5.6 Sol, GPT-6 Sol, GPT-6.1 Sol and GPT-6.2 Sol are separate models. When you read a claim about "Sol", look for the version. Without it, the claim could be about any of them.

## What is each GPT-6 tier built for?

OpenAI's latest-model guide now covers GPT-6.2 Astra and GPT-6.2 Sol. Its GPT-6 and GPT-6.1 section describes the older tiers this way:

- **GPT-6 Astra** : For the most demanding work.
- **GPT-6.1 Sol** : For complex work at lower cost.
- **GPT-6.1 Terra** : For visual reasoning and writing.
- **GPT-6 Luna** : For focused, high-volume tasks.

In practice, pick the tier by the size of the job. Long reasoning and hard code are what Astra is for. Everyday writing and most coding questions are Sol's job. Short, repeated tasks such as sorting messages or rewriting a sentence fit Luna, because Luna is the fastest and the cheapest tier. Terra joined Work and Codex on October 26, 2026. OpenAI's Work and Codex help article lists GPT-6.1 Terra among the options available depending on plan and workspace settings, and says it does not appear in the regular Chat picker.

OpenAI's model page for Astra lists a context window (the amount of text the model can read in one request) of 1,050,000 tokens, a maximum output of 128K tokens, and a knowledge cutoff of April 30, 2026. A token is a piece of a word, and API prices are counted in tokens.

## Which GPT-6 model does my ChatGPT plan use?

This is the question most chat users have. OpenAI's ChatGPT model help page, checked on November 19, 2026, gives the current answer. When GPT-6 is selected in Chat, it says "GPT-6 Sol powers Instant through Extra High where available." In short:

- Free and Go plans use GPT-5.6 Luna. The page says "Free and Go users receive GPT-5.6 Luna" and lists no GPT-6 model for them in Chat.
- Plus, Pro, Business and Enterprise use GPT-6, powered by GPT-6 Sol, in the chat window. Plus gets the Medium and High thinking levels, not Extra High or Pro.
- The "Pro" reasoning option runs on Astra: "GPT-6 Pro, powered by GPT-6 Astra, is available in ChatGPT for Pro $100, Pro $200, Business, and Enterprise plans."

The ChatGPT release notes, also checked on November 19, 2026, tell an older story. The October 7 entry says Plus, Pro, Business and Enterprise use GPT-6 Sol and that Free and Go use GPT-6 Luna, with Free and Go following one day after the paid plans. The model help page does not mention GPT-6 Luna at all, so do not count on it being in Chat on a free plan. For Enterprise, availability depends on admin settings. To check what your own account uses today, open the model menu in ChatGPT.

If you are on a free plan and also wonder about ads, that depends on the plan too, and it is covered in which ChatGPT plans show ads.

## Codex and ChatGPT Work run different versions

The new models did not reach every OpenAI product in the same form. In Chat, the GPT-6 option runs GPT-6 Sol. Work and Codex also offer a newer Sol. OpenAI's Work and Codex help article, checked on November 19, 2026, now directs users to choose GPT-6.2 Astra or GPT-6.2 Sol in the Work model picker or Codex model selector. Available options depend on plan, seat and workspace settings. GPT-6.1 Sol, GPT-6.1 Terra, GPT-6 Sol and GPT-6 Luna remain options you may see depending on your plan and workspace. Plus includes limited GPT-6 Astra usage in Work and Codex; Pro $100 and Pro $200 can use their full existing Work and Codex allowance for GPT-6 Astra. GPT-6.2 Astra and Sol require Codex CLI version 0.166.0 or later.

The timing explains the gap. A September 29, 2026 release notes entry brought GPT-6.1 Sol to Work and Codex, starting with Pro users and then expanding to Plus, Business, Enterprise and Edu. The October 7 release changed Chat but, per the release notes, did not change the models behind Work and Codex. The October 7 safety page adds that users of GPT-6 Sol and GPT-6 Luna in Codex and Work "are still using previously released versions". GPT-6.1 Sol sits beside those two as its own option. It does not replace GPT-6 Sol there, and OpenAI's Codex models page also uses gpt-6.1-sol and gpt-6-sol as separate model IDs. Terra arrived on October 26, 2026, when the release notes said "GPT-6.1 Terra is rolling out in ChatGPT Work and Codex." The Codex models page adds: "GPT-6.1 Terra requires Codex CLI 0.159.0 or later." The newest additions, GPT-6.2 Astra and GPT-6.2 Sol, arrived on November 17, 2026, in preview in Work and Codex desktop. Regular Chat keeps its existing model choices.

So "Sol" in Codex can mean GPT-6.2 Sol, GPT-6.1 Sol or GPT-6 Sol, and the name alone does not identify the same build as Sol in the chat window. If a result in Codex differs from a result in chat, check which Sol your Codex session uses; you can pick the model with `/model` in an interactive session. If you run Codex through your ChatGPT plan, using Codex with a ChatGPT subscription explains how that setup works.

## GPT-6 API prices for Astra, Sol and Luna

ChatGPT plans have a monthly price. The API charges per token instead. These are the standard-tier list prices on OpenAI's API pricing page and individual model pages, checked on November 19, 2026. All prices are in US dollars per 1 million tokens, for prompts of 272K input tokens or fewer. The charts retain the GPT-6 and GPT-6.1 models compared here. The newer GPT-6.2 Astra costs $8 for input and $40 for output at Standard rates; GPT-6.2 Sol costs $2 and $10.

## The data behind this chart

```
[
  {
    "label": "gpt-6-astra",
    "input_usd": "10.00",
    "cached_input_usd": "1.00",
    "cache_write_usd": "12.50",
    "output_usd": "50.00"
  },
  {
    "label": "gpt-6.1-sol",
    "input_usd": "2.00",
    "cached_input_usd": "0.10",
    "cache_write_usd": "2.50",
    "output_usd": "10.00"
  },
  {
    "label": "gpt-6-sol",
    "input_usd": "2.00",
    "cached_input_usd": "0.20",
    "cache_write_usd": "2.50",
    "output_usd": "10.00"
  },
  {
    "label": "gpt-6.1-terra",
    "input_usd": "1.00",
    "cached_input_usd": "0.05",
    "cache_write_usd": "1.25",
    "output_usd": "5.00"
  },
  {
    "label": "gpt-6-luna",
    "input_usd": "0.10",
    "cached_input_usd": "0.01",
    "cache_write_usd": "0.125",
    "output_usd": "0.50"
  }
]
```
Astra costs **$10.00** per million input tokens and **$50.00** per million output tokens. Luna costs **$0.10** for input and **$0.50** for output. Both Sol models sit between them at **$2.00** for input and **$10.00** for output. The new GPT-6.1 Terra costs **$1.00** for input and **$5.00** for output, which puts it between Sol and Luna.

Output tokens cost five times as much as input tokens on every model in the table. That matters because a model that writes long answers costs more than one that only reads long prompts.

Cached input is the lower price for input the API has already seen in a recent request, such as a long system prompt that repeats. Cache writes are the price for storing that input the first time. The cache-write figures shown use 30-minute retention; 12-hour retention costs more. The pricing page also lists Batch, Flex, Fast and Ultrafast tiers with different rates. This guide covers only the standard tier.

## Why does the API list two Sol models?

The API has both gpt-6-sol and gpt-6.1-sol. Their input and output prices are the same. The difference in the table is cached input: GPT-6.1 Sol charges **$0.10** per million cached tokens, and GPT-6 Sol charges **$0.20**.

OpenAI's latest-model guide now focuses on GPT-6.2 Astra and GPT-6.2 Sol, and its migration guidance targets GPT-6.2 Astra. The GPT-6 and GPT-6.1 section still explains switching from gpt-6-sol to gpt-6.1-sol. That section also says gpt-6-sol supports a reasoning effort of `none`, which GPT-6 Astra and GPT-6.1 Sol do not.

For chat users, OpenAI's model help page says GPT-6 Sol powers the GPT-6 option in Chat, while the Work and Codex help article lists GPT-6.1 Sol among the separate options there. So do not assume the chat window runs the 6.1 build.

## What does one request cost on each model?

Per-million prices are hard to picture. This chart applies the short-context prices above to one example request: 10,000 input tokens and 1,000 output tokens, with no caching. That is roughly a long document plus a one-page answer. The figures are our arithmetic from OpenAI's list prices, not a number OpenAI publishes.

## The data behind this chart

```
[
  {
    "label": "gpt-6-astra",
    "input_cost_usd": "0.10",
    "output_cost_usd": "0.05",
    "total_usd": "0.15"
  },
  {
    "label": "gpt-6.1-sol",
    "input_cost_usd": "0.02",
    "output_cost_usd": "0.01",
    "total_usd": "0.03"
  },
  {
    "label": "gpt-6-sol",
    "input_cost_usd": "0.02",
    "output_cost_usd": "0.01",
    "total_usd": "0.03"
  },
  {
    "label": "gpt-6.1-terra",
    "input_cost_usd": "0.01",
    "output_cost_usd": "0.005",
    "total_usd": "0.015"
  },
  {
    "label": "gpt-6-luna",
    "input_cost_usd": "0.001",
    "output_cost_usd": "0.0005",
    "total_usd": "0.0015"
  }
]
```
The request costs **$0.15** on Astra, **$0.03** on Sol, **$0.015** on Terra and **$0.0015** on Luna. One request is cheap on any of them. The gap matters at volume: a script that sends the same request thousands of times a day pays for every one of them, so the tier choice decides most of the bill.

## Prompts over 272K tokens cost more

For the models in these charts, the model pages apply higher rates above 272K input tokens. For Astra, OpenAI's model page words it this way: "Prompts with more than 272K input tokens are priced at 2x input and cache rates and 1.5x output for the full request." The key words are "the full request". The higher rate then covers every token in that request, including the first 272K.

## The data behind this chart

```
[
  {
    "label": "gpt-6-astra",
    "input_usd": "20.00",
    "cached_input_usd": "2.00",
    "cache_write_usd": "25.00",
    "output_usd": "75.00"
  },
  {
    "label": "gpt-6.1-sol",
    "input_usd": "4.00",
    "cached_input_usd": "0.20",
    "cache_write_usd": "5.00",
    "output_usd": "15.00"
  },
  {
    "label": "gpt-6-sol",
    "input_usd": "4.00",
    "cached_input_usd": "0.40",
    "cache_write_usd": "5.00",
    "output_usd": "15.00"
  },
  {
    "label": "gpt-6.1-terra",
    "input_usd": "2.00",
    "cached_input_usd": "0.10",
    "cache_write_usd": "2.50",
    "output_usd": "7.50"
  },
  {
    "label": "gpt-6-luna",
    "input_usd": "0.20",
    "cached_input_usd": "0.02",
    "cache_write_usd": "0.25",
    "output_usd": "0.75"
  }
]
```
In the long band, Astra input rises to **$20.00** and output to **$75.00**. The table shows the same pattern for Sol, Terra and Luna: input doubles and output rises by half. This is an API pricing rule. The pricing page does not say how ChatGPT plans handle very long conversations.

## Does a ChatGPT subscription include API access?

This question comes up often, and the answer depends on the plan. OpenAI's API credits benefit help article, checked on November 19, 2026, confirms that eligible paid Pro and Business subscribers can activate promotional API credits for a linked API organization. The eligible subscription must have been active for at least seven days. Plus does not include API usage or qualify for this benefit. ChatGPT and API billing remain separate.

The API pricing page also lists a model called chat-latest under a ChatGPT heading, with its own per-token API price. That is an API model with an API price. It is not a description of a ChatGPT plan. For OpenAI models used through cloud platforms, the page says they "are billed through those services", naming Amazon Bedrock and Microsoft Azure.

If you plan to call the API, budget from the per-token prices above. If your subscription is eligible for API credits, activate the benefit and check the grant and expiry in the receiving API organization. Unused promotional credits expire at the end of the subscription billing cycle and do not roll over. These credits do not cover Codex usage through API keys.

## What about message limits and the Pro plan?

This guide does not quote message caps for Astra, Sol or Luna in ChatGPT. OpenAI's model help page, checked on November 19, 2026, describes limits only in words, such as a weekly limit or a daily allowance, with no numbers. It does say "Free and Go users have unlimited everyday text chats, subject to abuse-prevention safeguards", with separate limits for file uploads, images, voice and data analysis. On Pro $200, reaching the weekly GPT-6 Pro limit switches you to GPT-5.6 Thinking at Medium. Secondhand cap figures go out of date quickly.

Pro plan usage has changed twice since late September 2026. A September 29, 2026 entry in the ChatGPT release notes introduces a Pro 500 plan at $500 a month, which has the most included usage of the Pro plans. Another entry the same day says Pro 200 is open to new subscribers again at $200 a month, that its included usage allowances are changing, and that affected users will get an email. An October 29, 2026 entry reverses that change: "We've also canceled the Pro 200 reduction announced in our September 29 email." The same entry says "Pro 100 and Pro 500 plans now include twice their previous Work and Codex allowance", and Pro 200 subscribers on the lower allowance also get twice their previous Work and Codex allowance. The entries do not give the limits as numbers. If usage limits are your reason to choose Pro, read the limits in OpenAI's help center on the day you subscribe. For a plan-level comparison with Anthropic's top consumer plan, see Claude Max vs ChatGPT Pro.

## How does GPT-6 compare with Claude on cost?

If you also pay for Claude, the useful comparison is cost. For the per-token side, Claude vs ChatGPT API pricing compares the two APIs. For the monthly side, what a Claude or ChatGPT subscription costs compares the paid plans, and the free plans of Claude, ChatGPT and Gemini covers what you get without paying.

## FAQ

### Is GPT-6 Sol better than GPT-6 Astra?

No. In the GPT-6 family, Astra is the top tier. OpenAI's latest-model guide now recommends GPT-6.2 Astra for the most demanding tasks and GPT-6.2 Sol for complex work at lower cost. Guides that call Sol the best OpenAI model were usually written for the GPT-5.6 lineup, which GPT-6 has been replacing on paid ChatGPT plans since October 2026. Sol is the better choice when you want lower cost and faster answers for everyday work.

### Which GPT-6 model do free ChatGPT users get?

None in the chat window, as of November 19, 2026. OpenAI's ChatGPT model help page says "Free and Go users receive GPT-5.6 Luna" and that GPT-5.6 Luna is becoming their default model, while GPT-6 is rolling out to eligible Plus, Pro, Business and Enterprise users. The October 7 release notes entry said Free and Go would use GPT-6 Luna, but the current model page does not list it. Check the model menu in your ChatGPT app to see what your account uses today.

### Why does Codex give different answers from ChatGPT with the same model name?

They may not be the same build. In Chat, the GPT-6 option runs GPT-6 Sol. OpenAI's Work and Codex help article now directs users to GPT-6.2 Astra or GPT-6.2 Sol, with GPT-6.1 Sol, GPT-6.1 Terra, GPT-6 Sol and GPT-6 Luna also appearing depending on plan and workspace settings. OpenAI's October 7 safety page says the GPT-6 Sol and GPT-6 Luna there are previously released versions. Plus and Pro plans also get GPT-6 Astra there. So Sol in Codex and Sol in the chat window can differ.

### How much does GPT-6 cost in the API?

At standard list prices for prompts up to 272K input tokens, Astra costs **$10.00** per million input tokens and **$50.00** per million output tokens. Both Sol models cost **$2.00** and **$10.00**. GPT-6.1 Terra costs **$1.00** and **$5.00**. Luna costs **$0.10** and **$0.50**. Prompts above 272K input tokens are billed at higher long-context rates for the whole request. OpenAI's API pricing page has the current figures.

### Does ChatGPT Plus include OpenAI API credits?

No. OpenAI's Plus help article says API usage is separate and billed independently. Plus is also ineligible for the promotional API credit benefit available to eligible paid Pro and Business subscriptions.
