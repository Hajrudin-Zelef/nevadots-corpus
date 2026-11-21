---
id: collect-261121-veille-ia/actu-business/stripe-completes-openrouter-acquisition-aitoolranked
title: "Stripe completes OpenRouter acquisition | AIToolRanked"
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-11-10"]
keywords: ["openrouter", "stripe", "august", "said", "november", "billion", "times", "same"]
source: https://aitoolranked.com/news/stripe-completes-openrouter-acquisition
source_anchor: ""
source_lines: [1, 45]
section: "Actu_Business"
---
# Stripe completes OpenRouter acquisition

Stripe and OpenRouter said on November 10 that the acquisition has closed. Neither company disclosed a price. The roughly $7.5 billion figure is a New York Times report, via TechCrunch and Payments Dive.

On **November 10, 2026 at 19:42 UTC**, Stripe posted ↗: "OpenRouter is now part of Stripe." Stripe said the combination will "help businesses explore new models and optimize token routing and usage." At **19:45 UTC**, OpenRouter posted ↗ that it is "officially part of @stripe today," and that more than 10 million developers and companies use it.

The agreement is older. On **August 19**, Stripe's newsroom said it has agreed to acquire OpenRouter ↗, described there as routing usage across **400+ models from more than 80 providers**, with NVIDIA, Zoom, and Lovable named as users. Patrick Collison said in that release: "Tokens are the central currency for companies building with AI." Alex Atallah said developers "need a neutral layer to orchestrate and manage them all."

OpenRouter's August 19 post ↗ said the deal was "subject to customary closing conditions" and expected to close "in the coming weeks." Fetched on November 11, that page still carried the August 19 text, including the closing-conditions line. The August 19 newsroom URL still reads as an agreement, not as a completed close. The November 10 posts are the completion notice.

**Neither company has disclosed a price.** About **$7.5 billion** is what reporters attributed to the New York Times, not a number Stripe printed.

TechCrunch on August 19 ↗ (Julie Bort): "While the company didn't disclose the deal price, sources told the New York Times that it paid $7.5 billion." Citing the Times, TechCrunch also reported $1.5 billion for the founders and $6 billion for investors. Payments Dive ↗ (Justin Bachman, August 19) reported the same Times figure and wrote that a Stripe spokesperson declined to comment on terms.

On August 16, before the agreement, TechCrunch ↗ reported a Bloomberg account of a deal above $7 billion, and said Stripe would not comment on rumors. The August 16 Bloomberg figure and the August 19 Times figure are separate press reports. They are not a Stripe disclosure.

OpenRouter's August 19 post, a company claim rather than a new audit, said it processed 10+ trillion tokens a day and called itself a 90-person startup. The November 10 post repeats "more than 10 million" developers and companies and does not restate the token figure.

August 19 is the product promise: "same mission, same name, same product, same roadmap. If you build on OpenRouter today, nothing about your integration changes." Routing, it said, "doesn't bend to any model, any provider, or any parent company."

November 10 restates the policy: "Our commitment to neutrality stays the same. Models and providers compete on quality, reliability, and price. You decide what works for your business." The post also says customers need control over cost and over where data goes.

The close posts do not announce an API break or a fee change. OpenRouter's FAQ ↗, fetched November 11, still charges **5.5% ($0.80 minimum)** on credit purchases, **5%** on crypto, and no markup on the provider's token price. Bring-your-own-key use above the pay-as-you-go allowance is another 5%.

Two product notes landed with the close. Both are vendor claims.

The Extractions API ↗ guide, by William Kofski and dated November 10, says the API is live **in alpha**. OpenRouter's November 11 post ↗ points at that guide: describe fields, get structured values from text, images, and PDFs. The guide names three extraction models, from Cloudflare, Inception, and ScaleDown, and shows calls to `https://openrouter.ai/api/alpha/extractions`. We did not run the sample.

On November 11 ↗, OpenRouter said that over the past month GLM 5.3 Flash users saw time-to-first-token fall 40%, throughput rise 10%, and dollars per million tokens drop 17%, from "our new routing algorithm." Those percentages are OpenRouter's, not an independent test. Our Auto Code router piece treats that family's scores the same way.

"OpenRouter being neutral was the whole point," wrote Merktop ↗, replying to Stripe.

"OpenRouter says its commitment to neutrality stays the same. That is a promise, not a structure," wrote BroadwakeAI ↗.

"One controls how AI is consumed. The other controls how it's paid for," wrote Debojyoti ↗.

Reddit was not reachable for this story.

**A blog promise is not a routing firewall.** OpenRouter says the name, product, roadmap, and integration stay, and that routing does not bend to a parent. Stripe's close post says the point is to "optimize token routing and usage." The company that can prefer a provider and the company that settles the bill are now one company. The November 10 posts describe no outside audit of routing and no contractual ban on favoring a provider. Neutrality here is a stated policy.

**The fee is the part you can check.** The FAQ still adds 5.5% when you buy credits, then passes provider prices through. Calling Anthropic, OpenAI, or Google directly skips that fee and also skips OpenRouter's fallbacks. That trade existed before the close. Cheaper Claude Code alternatives and the coding-tool guide are where a known model beats a router.

**Two other routes are documented products.** Vercel AI Gateway ↗ is a hosted API at `https://ai-gateway.vercel.sh`. Vercel says it adds zero markup to provider token prices, including bring-your-own-key, and that the app need not run on Vercel. Failover is still someone else's routing policy. One of those routes is the DeepSeek V4.1 Flash path served by Fireworks. LiteLLM ↗ is a self-hosted gateway. The repo describes one OpenAI-format interface to 100+ LLM APIs, and GitHub lists the license as Other. Self-hosting puts prompts on machines you run. The model vendor stays upstream.

**Stay for the unified bill. Split any model you will not let a router move.** Re-read which providers are allowed to log prompts. Muse Ember was on OpenRouter and Vercel AI Gateway the day it launched: the router sees a model early, and the lab's own API is still there. For weights you run yourself, start with the open-source LLM comparison.
