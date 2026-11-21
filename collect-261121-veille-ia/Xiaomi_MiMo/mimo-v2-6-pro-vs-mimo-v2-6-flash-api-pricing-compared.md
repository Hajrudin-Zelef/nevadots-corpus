---
id: collect-261121-veille-ia/xiaomi-mimo/mimo-v2-6-pro-vs-mimo-v2-6-flash-api-pricing-compared
title: "MiMo V2.6 Pro vs MiMo V2.6 Flash API pricing compared"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["mimo", "flash", "price", "cloud", "cheapest", "input", "standard", "tokens"]
source: https://broadrouter.com/llm-api-pricing/compare/mimo-v2.6-pro-vs-mimo-v2.6-flash/
source_anchor: ""
source_lines: [1, 116]
section: "Xiaomi_MiMo"
---
# MiMo V2.6 Pro vs MiMo V2.6 Flash API pricing

Updated

MiMo V2.6 Pro: USD 0.32625 input, USD 0.6525 output per 1M tokens (GMI Cloud, promotional). MiMo V2.6 Flash: USD 0.112 input, USD 0.224 output per 1M tokens (GMI Cloud, promotional). At 3 input tokens per output token, MiMo V2.6 Pro costs 2.91× as much as MiMo V2.6 Flash (USD 0.4078 vs USD 0.14 per 1M).

## How do MiMo V2.6 Pro and MiMo V2.6 Flash differ on price?

- Cheapest input price: MiMo V2.6 Flash USD 0.112 against USD 0.32625 per 1M tokens, so MiMo V2.6 Pro costs 2.9× as much.
- Cheapest output price: MiMo V2.6 Flash USD 0.224 against USD 0.6525 per 1M tokens, so MiMo V2.6 Pro costs 2.9× as much.
- Among the 200 models with two or more providers, MiMo V2.6 Pro ranks 83rd cheapest and MiMo V2.6 Flash 35th on the 3:1 blend.
- No input:output mix changes the answer: MiMo V2.6 Flash is cheaper at every mix before cache discounts.
- Both list cached-input discounts: MiMo V2.6 Pro at 3 standard prices of 3, from USD 0.0027 (GMI Cloud, 99% below that provider's input price); MiMo V2.6 Flash at 5 standard prices of 5, from USD 0.00224 (GMI Cloud, 98% below that provider's input price).
- Both state a context window of up to 1,050,000 tokens.
- MiMo V2.6 Flash has 5 providers with a standard price, 2 more than MiMo V2.6 Pro; 3 providers sell both (GMI Cloud, DeepInfra and Novita AI).
- On one account, GMI Cloud charges USD 0.4078 for MiMo V2.6 Pro (its cheapest price) and USD 0.14 for MiMo V2.6 Flash (its cheapest price) at 3:1, the lowest combined price among the providers that sell both.
- Shopping around matters more for MiMo V2.6 Flash: its dearest standard price is 1.56× its cheapest, against 1.33× for MiMo V2.6 Pro.
- For the one-developer coding agent preset, the gap is USD 1,165 a year in MiMo V2.6 Flash's favour (MiMo V2.6 Pro USD 150.94 a month, MiMo V2.6 Flash USD 53.83).

## MiMo V2.6 Pro and MiMo V2.6 Flash prices side by side

USD per 1M tokens, standard tier. "Cheapest" is the lowest price at 3 input tokens to 1 output token; each line can come from a different provider.

| MiMo V2.6 Pro and MiMo V2.6 Flash API prices compared |  |  | 
|---|---|---|
| Price | MiMo V2.6 Pro | MiMo V2.6 Flash | 
|---|---|---|
| Model page | MiMo V2.6 Pro Xiaomi · 3 providers | MiMo V2.6 Flash Xiaomi · 5 providers | 
| Cheapest standard price, blended 3:1 | USD 0.4078 promo USD 0.32625 in / USD 0.6525 out at GMI Cloud GMI Cloud page checked 2026-11-18 | USD 0.14 lower promo USD 0.112 in / USD 0.224 out at GMI Cloud GMI Cloud page checked 2026-11-18 | 
| Cheapest input | USD 0.32625 promo GMI Cloud source | USD 0.112 lower promo GMI Cloud source | 
| Cheapest cached input | USD 0.0027 promo GMI Cloud source | USD 0.00224 lower promo GMI Cloud source | 
| Cheapest output | USD 0.6525 promo GMI Cloud source | USD 0.224 lower promo GMI Cloud source | 
| Cheapest non-promotional price | USD 0.435 / USD 0.87 Novita AI, USD 0.5437 blended source | USD 0.14 / USD 0.28 DeepInfra, USD 0.175 blended source | 
| Maker's own API | Xiaomi publishes no paid standard price on its own API that we found. | Xiaomi publishes no paid standard price on its own API that we found. | 
| Median standard price | USD 0.387 / USD 0.783 input / output, 3 prices | USD 0.14 / USD 0.28 input / output, 5 prices | 
| Context window | 1,050,000 tokens, largest stated; from 1,048,576 at some providers | 1,050,000 tokens, largest stated; from 1,000,000 at some providers | 
| Providers with a standard price | 3 | 5 | 

- **Lower** marks the lower price on a line;**tie** , equal prices. Other tiers and free rows are on the model pages.
- **Promo:** the provider calls the price promotional or discounted, so it is current now and can end without notice.

Report a wrong price or number (email to info@broadnet.ai, with this page's link filled in).

## What do MiMo V2.6 Pro and MiMo V2.6 Flash cost for a real workload?

30 days at each model's cheapest standard price for the workload (presets of the cost calculator; cached input at the cached price where listed).

| Monthly cost of three workloads on MiMo V2.6 Pro and MiMo V2.6 Flash |  |  |  | 
|---|---|---|---|
| Workload | MiMo V2.6 Pro | MiMo V2.6 Flash | Difference | 
|---|---|---|---|
| **Support chatbot**2,000 requests a day, 1,500 input tokens (60% cached), 300 output | USD 23.64 GMI Cloud | USD 8.18 GMI Cloud | USD 15.45 MiMo V2.6 Flash lower | 
| **RAG over documents**1,000 requests a day, 8,000 input tokens (30% cached), 500 output | USD 64.79 GMI Cloud | USD 22.34 GMI Cloud | USD 42.45 MiMo V2.6 Flash lower | 
| **Coding agent (one developer)**1,500 requests a day, 40,000 input tokens (85% cached), 2,000 output incl. reasoning | USD 150.94 GMI Cloud | USD 53.83 GMI Cloud | USD 97.12 MiMo V2.6 Flash lower | 

MiMo V2.6 Flash is cheaper on all three workloads. The widest gap is on RAG over documents: the dearer model costs 2.9× as much as the cheaper one.

## Does the cheaper model change with your input:output ratio?

MiMo V2.6 Flash is cheaper at every mix of input and output tokens, before cache discounts: its cheapest input price and its cheapest output price are both at or below MiMo V2.6 Pro's, so the input:output ratio never changes the answer.

| Price per 1M tokens of MiMo V2.6 Pro and MiMo V2.6 Flash at different input:output mixes |  |  | 
|---|---|---|
| Input : output tokens | MiMo V2.6 Pro | MiMo V2.6 Flash | 
|---|---|---|
| **1 : 3** (long answers) | USD 0.5709 GMI Cloud | USD 0.196 lower GMI Cloud | 
| **1 : 1** | USD 0.4894 GMI Cloud | USD 0.168 lower GMI Cloud | 
| **3 : 1** (the site’s blend) | USD 0.4078 GMI Cloud | USD 0.14 lower GMI Cloud | 
| **10 : 1** (retrieval, long prompts) | USD 0.3559 GMI Cloud | USD 0.1222 lower GMI Cloud | 
| **30 : 1** (agents, big contexts) | USD 0.3368 GMI Cloud | USD 0.1156 lower GMI Cloud | 

USD per 1M tokens at the cheapest standard provider for each mix, before cache discounts. Thinking tokens bill as output.

## What do MiMo V2.6 Pro and MiMo V2.6 Flash cost per character in other languages?

Not measured: the real cost by language dataset covers only tokenizers we could measure from published files, and MiMo V2.6 Pro and MiMo V2.6 Flash are not among them. Count tokens on your own text with the counters below.

## Check it on your own traffic

- MiMo V2.6 Pro API pricing: all 3 providers and other tiers.
- MiMo V2.6 Flash API pricing: all 5 providers and other tiers.
- Cost calculator with MiMo V2.6 Pro and MiMo V2.6 Flash preloaded
- Token counter
- Prompt caching calculator

More comparisons: Gemma 4 26B A4B vs MiMo V2.6 Flash · every MiMo V2.6 Pro comparison · every MiMo V2.6 Flash comparison · all 656 price comparisons

## Questions

## Is MiMo V2.6 Pro cheaper than MiMo V2.6 Flash?

No. On a blend of 3 input tokens to 1 output token, MiMo V2.6 Flash costs USD 0.14 per 1M tokens and MiMo V2.6 Pro USD 0.4078, at each model's cheapest standard price. For input alone, MiMo V2.6 Flash is cheaper (USD 0.112 against USD 0.32625 per 1M); for output alone, MiMo V2.6 Flash is (USD 0.224 against USD 0.6525). This compares price only, not quality.

## What do MiMo V2.6 Pro and MiMo V2.6 Flash cost per 1M tokens?

MiMo V2.6 Pro: from USD 0.32625 input and USD 0.6525 output at GMI Cloud (promotional); median of 3 standard prices USD 0.387 / USD 0.783. MiMo V2.6 Flash: from USD 0.112 input and USD 0.224 output at GMI Cloud (promotional); median of 5 standard prices USD 0.14 / USD 0.28. Each price is read from the provider's own pricing page; the source is linked in the table.

## Which providers sell both MiMo V2.6 Pro and MiMo V2.6 Flash?

3 providers have a standard price for both: GMI Cloud (USD 0.4078 and USD 0.14 at 3:1), DeepInfra (USD 0.486 and USD 0.175 at 3:1), Novita AI (USD 0.5437 and USD 0.175 at 3:1). The first is the lowest combined price. Buying each model from its own cheapest provider costs USD 0.4078 and USD 0.14.

## Which is cheaper for a coding agent or a chatbot, MiMo V2.6 Pro or MiMo V2.6 Flash?

For a coding agent (1,500 requests a day, 40,000 input tokens (85% cached), 2,000 output incl. reasoning): USD 150.94 a month on MiMo V2.6 Pro (GMI Cloud) against USD 53.83 on MiMo V2.6 Flash (GMI Cloud), a difference of USD 97.12. For a support chatbot (2,000 requests a day, 1,500 input tokens (60% cached), 300 output): USD 23.64 a month on MiMo V2.6 Pro (GMI Cloud) against USD 8.18 on MiMo V2.6 Flash (GMI Cloud), a difference of USD 15.45. Each uses the cheapest standard price for that workload, cached input included. Try your own numbers.

## At what input:output ratio does the cheaper of MiMo V2.6 Pro and MiMo V2.6 Flash change?

MiMo V2.6 Flash is cheaper at every mix of input and output tokens, before cache discounts: its cheapest input price and its cheapest output price are both at or below MiMo V2.6 Pro's, so the input:output ratio never changes the answer.

## Do MiMo V2.6 Pro and MiMo V2.6 Flash have cached input pricing?

MiMo V2.6 Pro from USD 0.0027 per 1M cached input tokens (GMI Cloud; 3 of 3 standard prices list a discount). MiMo V2.6 Flash from USD 0.00224 per 1M cached input tokens (GMI Cloud; 5 of 5 standard prices list a discount). Cached tokens are billed at the input price where no cache price is published.

## Which has the larger context window, MiMo V2.6 Pro or MiMo V2.6 Flash?

Both go up to 1,050,000 tokens on the providers that state it.
