---
id: collect-261121-veille-ia/claude-anthropic/claude-on-openrouter-what-you-actually-pay-ssd-nodes
title: "Claude on OpenRouter: what you actually pay · SSD Nodes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["anthropic", "openrouter", "opus", "output", "cache", "input", "endpoints", "price"]
source: https://www.ssdnodes.com/learn/claude-on-openrouter-what-you-pay
source_anchor: ""
source_lines: [1, 201]
section: "Claude_Anthropic"
---
## What Claude on OpenRouter costs

Claude on OpenRouter costs Anthropic's list price per token, because OpenRouter states that it adds no markup on inference. The extra cost is a fee you pay when you buy credits. As of November 2026, the published plan rates are 5.5% on Standard and 8% on Business. Standard non-crypto purchases have a $0.80 minimum fee. OpenRouter's fee announcement lists crypto payments at 5%, with no minimum fee.

So Opus 5.5 on OpenRouter's standard Anthropic line costs $4 per million input tokens and $20 per million output tokens. Anthropic's own price list shows the same numbers. OpenRouter charges the fee when you top up your balance. Its pricing page says the fee is "never on individual requests", so a single API call costs the same on every plan. This guide works through one real job, compares the three ways to pay for it, and covers the credit rules that change the final number.

Prices below are per MTok (one million tokens). If you are not sure how much text that is, read how much text one million Claude tokens holds first. Every price here was checked on 16 November 2026.

## Opus 5.5 and Fable 5.2 prices on OpenRouter

The model page for Opus 5.5 (`anthropic/claude-opus-5.5`) lists several provider lines with different prices. The page now labels providers and fast endpoints. OpenRouter's public endpoints API also identifies them, and the Opus lines in the chart below come from it.

## The data behind this chart

```
[
  {
    "label": "Opus 5.5, Anthropic and global cloud endpoints",
    "input_usd": "4.00",
    "output_usd": "20.00",
    "cache_read_usd": "0.20",
    "cache_write_usd": "5.00"
  },
  {
    "label": "Opus 5.5, US and EU regional endpoints",
    "input_usd": "4.40",
    "output_usd": "22.00",
    "cache_read_usd": "0.22",
    "cache_write_usd": "5.50"
  },
  {
    "label": "Opus 5.5, Anthropic fast (priority tier)",
    "input_usd": "8.00",
    "output_usd": "40.00",
    "cache_read_usd": "0.40",
    "cache_write_usd": "10.00"
  },
  {
    "label": "Fable 5.2, standard endpoints",
    "input_usd": "8.00",
    "output_usd": "40.00",
    "cache_read_usd": "0.20",
    "cache_write_usd": "10.00"
  },
  {
    "label": "Fable 5.2, Anthropic fast (priority tier)",
    "input_usd": "16.00",
    "output_usd": "80.00",
    "cache_read_usd": "0.40",
    "cache_write_usd": "20.00"
  }
]
```
The first Opus line is **$4.00** for input and **$20.00** for output. You get this price on Anthropic, Amazon Bedrock (global), Azure (global), Google Vertex (global) and Claude Platform on AWS. It matches Anthropic's list price for Opus 5.5.

The second line, **$4.40** and **$22.00**, is for the regional endpoints: Bedrock in `us-east-1` and `eu-west-1`, Azure US, and Vertex US and Europe. That is 10% more. Anthropic's own pricing page describes the same 10% premium for regional endpoints on Bedrock and Google Cloud. Anthropic's API also charges a 1.1x multiplier when you ask for US-only inference with the `inference_geo` parameter.

The third line is the one that confuses people. The Opus 5.5 page shows an "Anthropic Fast" line at **$8.00** input and **$40.00** output, which looks like double the price. The endpoints API tags it `anthropic/fast` and sets `service_tier` to `priority`. This line is fast mode, the faster output tier that Anthropic also sells at $8/$40 for Opus 5.5. It is not the normal Anthropic price.

Fable 5.2 (`anthropic/claude-fable-5.2`) costs **$8.00** for input and **$40.00** for output on its standard lines. Its model page lists a 1,000,000-token context window. Its fast line is **$16.00** and **$80.00**. Both match Anthropic's list prices. OpenRouter currently lists Fable's Amazon Bedrock endpoint as BYOK-only. To see what Fable is good at, and when Opus is enough, read Fable's price and the jobs it is worth paying for.

## Check the live prices yourself

Prices and providers change. The endpoints API is public and needs no key, so you can read the current lines from any server.

```
sudo apt update && sudo apt install -y curl jq
curl -s https://openrouter.ai/api/v1/models/anthropic/claude-opus-5.5/endpoints \
  | jq -r '.data.endpoints[] | [.tag, .service_tier, .pricing.prompt, .pricing.completion, .pricing.input_cache_read, .pricing.input_cache_write] | @tsv'
```
Each line prints the endpoint tag, the service tier and four prices in USD **per token**. Multiply by 1,000,000 to get the price per MTok. For Opus 5.5, a healthy result includes a line starting with `anthropic` that shows `0.000004` and `0.00002`. Those are $4 and $20 per MTok. A line with `priority` in the second column is fast mode. If the output is empty or `jq` prints an error, open the URL in a browser and check the model id. Fable's model page also shows its current provider prices.

## Does prompt caching pass through on OpenRouter?

Yes. The Opus 5.5 endpoints API lists cache read, 5-minute cache write and 1-hour cache write prices, and they match Anthropic's. On the standard Opus 5.5 line, a cache read costs **$0.20** per MTok and a cache write costs **$5.00**. Anthropic lists the same two numbers for Opus 5.5. The write figure is its 5-minute write price.

Two details matter. First, use each model's own price, not the general rule. OpenRouter's caching guide says reads cost 0.1x the input price. An Opus 5.5 read costs 0.05x ($0.20 against $4), and a Fable 5.2 read costs 0.025x ($0.20 against $8). Second, the API shows both write prices: `input_cache_write` for 5 minutes and `input_cache_write_1h` for 1 hour. Anthropic charges $8 per MTok for a 1-hour cache write on Opus 5.5. The standard endpoints show that same rate as `0.000008` per token. Caching pays off only if a prefix is reused often enough. The prompt caching break-even point shows how to work that out.

## Is the 5.5% fee added on top or taken from the top-up?

Secondary guides disagree on this. As of 16 November 2026, OpenRouter's pricing page and FAQ give the percentages. Its terms give no fee rate at all. None of them says whether the fee is added to what you pay or taken out of the credits you receive. For non-crypto payments, OpenRouter's platform-fee announcement gives "5.5% of the order amount, with a minimum fee of $0.80" and shows a $5.50 fee on a $100 order. It also does not say whether that order gives you $100 or $94.50 of credits.

So this guide does not choose one reading. Before you rely on a number, look at the checkout screen on your first top-up, which shows what you pay and what goes into your balance. The two readings are close. On Standard non-crypto purchases above the minimum-fee threshold, the fee is either 5.5% or about 5.8% of what you spend on tokens.

The $0.80 minimum matters only for small non-crypto top-ups. On a $10 non-crypto Standard order, 5.5% would be $0.55. The minimum applies instead, so the fee is really 8%. Crypto payments have a 5% fee with no minimum.

## A worked example: one month of Opus 5.5

Take a support assistant that sends 50 million input tokens and receives 5 million output tokens in a month, with no caching. At list price, that is 50 x $4 plus 5 x $20, or **$300.00**. The chart shows what each way to pay costs for the same tokens. OpenRouter does not say which way the credit fee works, so the fee rows show both readings and label them as assumptions. The credit rows assume non-crypto purchases.

## The data behind this chart

```
[
  {
    "label": "Anthropic API direct",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "0.00",
    "total_usd": "300.00"
  },
  {
    "label": "OpenRouter BYOK, under the monthly allowance",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "0.00",
    "total_usd": "300.00"
  },
  {
    "label": "Standard credits, if fee is added on top",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "16.50",
    "total_usd": "316.50"
  },
  {
    "label": "Standard credits, if fee is taken from the top-up",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "17.46",
    "total_usd": "317.46"
  },
  {
    "label": "Business credits, if fee is added on top",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "24.00",
    "total_usd": "324.00"
  },
  {
    "label": "Business credits, if fee is taken from the top-up",
    "list_price_usd": "300.00",
    "openrouter_fee_usd": "26.09",
    "total_usd": "326.09"
  }
]
```
If the fee is added on top, Standard credits cost **$316.50**. If the fee is taken from the top-up, you must buy $300 / 0.945 of credits to end up with $300. That costs **$317.46**. On Business the total is between **$324.00** and **$326.09**. Both fee-free routes cost **$300.00**.

The fee is the same percentage on every model. That means the model and the cache hit rate change the bill more than the way you pay. Moving this job to the regional $4.40/$22 line adds 10% to the token bill, which costs more than the whole Standard fee.

## Three ways to pay for the same Claude tokens

- **Credits through OpenRouter.** You prepay a balance and OpenRouter pays the provider. Tokens cost list price. Published credit purchase fees are 5.5% (Standard) or 8% (Business), with a $0.80 minimum on Standard non-crypto purchases; the fee announcement lists crypto at 5% with no minimum. Enterprise has fee discounts.
- **BYOK through OpenRouter.** BYOK means bring your own key. You add your own Anthropic API key, and Anthropic bills you directly for the tokens. On Standard and Business, the first $25,000 of list-price BYOK inference each month has no OpenRouter fee. Above that amount, OpenRouter charges 5% of what the same request would cost at list price on OpenRouter. It takes that fee from your credit balance. This allowance does not waive the purchase fee on credits used for ordinary OpenRouter inference.
- **Anthropic's API directly.** There is no router and no platform fee. Anthropic's pricing page says billing is based on actual monthly usage. Anthropic also sells the Batch API at $2/$10 per MTok for Opus 5.5, which is half the standard price, for work that can wait.

For a closer look at the direct route and how its per-token prices compare with another vendor's, read Claude and ChatGPT API pricing side by side.

## When BYOK is the cheaper OpenRouter path

With BYOK on Standard or Business, you pay no OpenRouter BYOK usage fee unless your list-price BYOK inference goes over $25,000 in a month. Above that allowance, the 5% fee comes out of your credits, so you still need a credit balance. You buy those credits like any others, which means the purchase fee applies to them too.

Check two BYOK settings. First, fallback. By default, if every one of your keys hits a rate limit or fails, OpenRouter falls back to its shared endpoints. Those requests then run on OpenRouter's shared capacity instead of your key. If every request must stay on your own key, turn on the option that never uses shared capacity for that provider and restrict requests to providers where you have configured your keys. Second, data regions. In OpenRouter's BYOK settings, Anthropic keys support only the Global and United States regions.

OpenRouter has announced that it will remove the 5% BYOK usage fee and replace it with a fixed monthly subscription. It has not set a price. The change is not in effect yet. On 16 November 2026 the pricing page still shows the $25,000 allowance and the 5% fee above it.

## Why people choose OpenRouter anyway

The fee buys some practical things.

- **One key and one bill for many models.** You can switch between Claude and other vendors' models by changing the model string, and you keep one balance.
- **Fallback across providers.** OpenRouter's FAQ says that if a provider returns an error, OpenRouter falls back to the next one. Anthropic, Bedrock, Azure, Vertex and Claude Platform on AWS all serve Opus 5.5, so OpenRouter can retry another eligible provider during an outage. The regional $4.40/$22 endpoints are skipped on the global API unless explicitly targeted; regional API requests use endpoints in their designated region.
- **EU in-region routing on Business and Enterprise.** Requests sent to`eu.openrouter.ai` are decrypted within Europe (the EU/EEA, the UK, and Switzerland) and routed only to shared provider endpoints in that region. Standard does not include this. For Opus 5.5, the European endpoints (Bedrock`eu-west-1` and Vertex Europe) both use the $4.40/$22 line, and Business lists an 8% credit purchase fee.

You can also get one endpoint for many models without a platform fee by running the router yourself with your own provider keys. A self-hosted LiteLLM gateway on a VPS does that job, but you must run and update it yourself.

## What credits cost you beyond the fee

Prepaid credits come with rules that a direct monthly bill does not have. These rules come from OpenRouter's terms, checked on 16 November 2026.

- **Unused credits can expire.** OpenRouter reserves the right to expire unused credits 365 days after purchase. This is a right OpenRouter keeps, not an automatic expiry. Still, plan as if it will happen, and buy only what you will use within a year.
- **The refund window is 24 hours.** You can ask for a refund of unused credits from non-crypto purchases within 24 hours of the transaction. After that, the ordinary purchase refund window closes.
- **Platform fees are never refunded** , even within the 24 hours. Cryptocurrency payments are never refundable.

Above the minimum-fee threshold, a larger top-up does not reduce the percentage fee. Small non-crypto Standard orders cost a higher effective percentage because of the $0.80 minimum. A bigger balance also has more to lose if credits expire.

## OpenRouter is not a way around Anthropic's country list

Anthropic's supported-countries page covers its products and its API. It also covers Claude models offered through cloud platforms and authorized resellers. All of these are available only in the countries and regions on the list. The rule covers people located in an unsupported region. It also covers companies incorporated or headquartered there. The policy effective 12 November 2026 also covers entities majority-owned or controlled, directly or indirectly, by people or entities in unsupported regions. A router in the middle does not change where you or your company are. Check the list before you build on any route to Claude.

## When a Pro, Max or Ultra plan beats all three

All three routes above bill per token. For chat in the Claude apps or for Claude Code, a subscription is often cheaper. The subscription price covers usage within the plan's included limits. If you enable usage credits, you can continue beyond those limits at standard API rates, charged in addition to the subscription. As of November 2026, claude.com/pricing lists Pro at $20 a month, or $17 a month on the annual plan ($200 billed up front). Max costs $100 a month for 5x Pro usage, and Ultra costs $200 a month for 10x Pro usage every week. Each plan includes Claude Code, which shares its usage limits with Claude. Max also includes $100 a month in Claude Platform API credits, and Ultra includes $200; these do not cover usage credits in the Claude apps or Claude Code. Watch for one trap. If `ANTHROPIC_API_KEY` is set in your shell, Claude Code asks you once whether to use that key and remembers your answer. If you approve it, or if you run Claude Code in non-interactive mode with `-p`, where the key is always used when present, you are billed at API rates instead of using your plan. If your monthly token bill for interactive work costs more than a plan, try the plan. When the Claude API costs less than a subscription has the full comparison. Use the API, through OpenRouter or direct, for apps, scripts and agents that run without a person at the keyboard.

## FAQ

### Does OpenRouter charge more than Anthropic for Claude tokens?

Not per token. OpenRouter states that inference is billed at the provider's list price on every plan. As of November 2026, Opus 5.5 on its standard Anthropic line costs $4 per million input tokens and $20 per million output tokens, the same as Anthropic. The extra cost is the fee you pay when you buy credits: published plan rates are 5.5% on Standard and 8% on Business, with a $0.80 minimum on Standard non-crypto purchases. OpenRouter's fee announcement lists crypto at 5% with no minimum.

### Is the OpenRouter credit fee added on top or taken out of my top-up?

As of 16 November 2026, OpenRouter's pricing page, FAQ and terms do not say. Look at the checkout screen on your first purchase. It shows what you pay and what goes into your balance. On Standard non-crypto purchases above the minimum-fee threshold, the two readings come to 5.5% or about 5.8% of what you spend on tokens. OpenRouter's platform-fee announcement also gives a minimum fee of $0.80 per non-crypto order and a 5% fee with no minimum for crypto payments.

### Do Claude prompt caching discounts work on OpenRouter?

Yes. OpenRouter's Opus 5.5 endpoints API lists cache read and both cache write prices, and they match Anthropic's. As of November 2026, on the standard Opus 5.5 line, a cache read costs $0.20 per million tokens, a 5-minute cache write costs $5, and a 1-hour cache write costs $8. Use each model's own price. OpenRouter's general caching guide says reads cost 0.1x the input price, but Opus 5.5 reads cost 0.05x.

### Why does the Opus 5.5 page on OpenRouter show an Anthropic price of $8/$40?

That line is fast mode. OpenRouter's endpoints API tags it `anthropic/fast` with the `priority` service tier. Anthropic sells the same fast mode for Opus 5.5 at $8 input and $40 output per million tokens. As of November 2026, the normal Anthropic line is $4/$20.

### Can I use OpenRouter to get Claude in a country Anthropic does not support?

Do not plan on it. Anthropic's supported-countries page says Claude models are available only in the countries and regions on its list. That includes models offered through cloud platforms and authorized resellers. The rule applies to people located in unsupported regions and to companies based there. It also covers entities majority-owned or controlled by people or entities in unsupported regions. Check that list before you choose any route.
