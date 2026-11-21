---
id: collect-261121-veille-ia/actu-business/what-stripe-s-7-billion-openrouter-deal-actually-means-for-ai-decrypt
title: "What Stripe's $7 Billion OpenRouter Deal Actually Means for AI - Decrypt"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["stripe", "openrouter", "billion", "deal", "more", "models", "million", "payments"]
source: https://decrypt.co/375769/what-stripe-openrouter-deal-means-ai
source_anchor: ""
source_lines: [1, 39]
section: "Actu_Business"
---
#### In brief

- Stripe finalized an agreement to acquire OpenRouter for more than $7 billion.
- OpenRouter routes traffic from around 8 million developers to more than 400 AI models and takes about 5% on the inference spend passing through it.
- Stripe already processed OpenRouter's payments, so the deal folds AI metering and AI billing into a single pipeline it owns end to end.

Stripe has finalized an agreement to buy OpenRouter for more than $7 billion, according to *Bloomberg*, three months after the AI routing startup raised $113 million at a reported $1.3 billion valuation.

*The Wall Street Journal* reported talks last month at a figure closer to $10 billion, so somebody negotiated.

OpenRouter's annualized revenue was around $50 million in March, per Sacra estimates. That puts the deal somewhere near 50 times revenue, which is not a multiple anyone pays for cash flow.

## What Stripe is buying

So what is Stripe, a company known for processing digital payments, actually buying? Position. OpenRouter sits between roughly 8 million developers and more than 400 AI models, giving them one API key instead of a dozen separate integrations. It owns no GPUs, trains nothing, and takes about 5% of the value paid per overall usage.

Stripe already handled OpenRouter's invoicing and tax, which makes this a vendor buying its own customer. Now the routing decision and the invoice sit inside the same company: OpenRouter picks which model answers a request and what it costs, Stripe collects on it.

It also hands Stripe a live read on enterprise AI spending across every major lab at once.

The pattern is not new. Stripe paid $1.1 billion for stablecoin firm Bridge and bought wallet infrastructure company Privy, then picked up usage-based billing startup Metronome in January 2026—a tool already used by OpenAI and Anthropic. It also co-built Tempo, whose Machine Payments Protocol lets AI agents request, authorize, and settle payments without a human in the loop.

## What developers stand to lose

Potentially, neutrality, or at least the easy version of it. OpenRouter's entire pitch was indifference: it routed to whatever model fit your budget, latency, and quality bar, with no stake in which one won. That was simple to believe when the owners were venture funds with no AI product of their own.

It reads differently when the owner meters traffic for labs it also sells payment services to. Stripe has not said whether OpenRouter will keep running as an independent product, get absorbed into its developer tools, or narrow to serve Stripe's own priorities. Right now, users should not feel a difference.

That said, there is a geopolitical wrinkle underneath. U.S.-origin models fell from roughly 70% of OpenRouter's token volume in mid-2025 to about 30% a year later, with cheap Chinese open-weight models absorbing the difference. Whoever sets the routing defaults has a hand on that dial.

## What it means for the labs

Less pricing power. Every time a developer trades an expensive flagship for a cheaper model that clears the bar, the router captures value and the lab loses a little leverage.

OpenRouter has been sharpening that blade itself. Its Fusion API fans a single prompt across a panel of budget models, then merges the answers. On DRACO, Perplexity's benchmark of 100 real deep-research tasks graded by an AI judge against expert rubrics and normalized to a percentage, a panel of Gemini 3 Flash, Kimi K2.6, and DeepSeek V4 Pro hit 64.7%, beating solo GPT-5.5 at 60% and solo Claude Opus 4.8 at 58.8%.

Stripe now owns that too.

Neither company has publicly confirmed the deal, and no regulatory review timeline has been disclosed. Stripe processed $1.9 trillion in payment volume in 2025 and was valued at $159 billion in a February tender offer. At more than $7 billion, OpenRouter is its largest acquisition to date—more than six times what it paid for Bridge.
