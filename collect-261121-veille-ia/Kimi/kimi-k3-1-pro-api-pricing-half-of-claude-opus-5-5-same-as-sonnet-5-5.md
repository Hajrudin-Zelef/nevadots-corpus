---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-api-pricing-half-of-claude-opus-5-5-same-as-sonnet-5-5
title: "Kimi K3.1 Pro API Pricing: Half of Claude Opus 5.5, Same as Sonnet 5.5"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-23"]
keywords: ["tokens", "kimi", "input", "output", "claude", "opus", "moonshot", "sonnet"]
source: https://dev.to/tokely/kimi-k31-pro-api-pricing-half-of-claude-opus-55-same-as-sonnet-55-fo4
source_anchor: ""
source_lines: [1, 70]
section: "Kimi"
---
**Short answer:** Moonshot AI released Kimi K3.1 Pro at $2 per 1M input tokens and $10 per 1M output tokens. That matches Claude Sonnet 5.5 and cuts Claude Opus 5.5 pricing in half. For long prompts without tiered pricing penalties, Kimi K3.1 Pro provides 1,048,576 tokens of context at a flat rate, alongside a 2 to 3 times faster Highspeed tier at double the price.

Moonshot AI released Kimi K3.1 Pro on October 23, 2026, as the first model of its K3.1 series. It is a post-trained successor to Kimi K3, which Moonshot released on July 16, 2026, as a 2.8-trillion-parameter open-weight model. Moonshot announced that the full model weights for K3.1 Pro will be released by November 4, 2026.

The release includes two API model IDs: kimi-k3.1-pro and kimi-k3.1-pro-highspeed. Moonshot removed cyber-attack material from training before the open release. On Moonshot's own benchmark, Kimi K3.1 Pro scored 65.7% on Terminal-Bench 4.0, compared to 66.4% for Claude Opus 5.5 and 58.2% for GPT-6 Astra. These benchmark numbers are Moonshot's own and not independently verified, and price is not capability, so test on your own prompts.

Kimi K3.1 Pro costs $2 per 1M input tokens on cache misses, $0.20 per 1M cached input tokens on cache hits, and $10 per 1M output tokens. Its predecessor, Kimi K3, lists at $3 per 1M input tokens, $0.30 per 1M cached input tokens, and $15 per 1M output tokens, making K3.1 Pro one third cheaper than the model it replaces.

The faster variant, Kimi K3.1 Pro Highspeed, lists at $4 per 1M input tokens, $0.20 per 1M cached input tokens, and $20 per 1M output tokens. Cache writes for both models are billed separately by time to live for either 5 minutes or 1 hour. The Vercel AI Gateway lists the cache write fee at $2 per 1M tokens for K3.1 Pro and $4 per 1M tokens for Highspeed.

Moonshot uses flat pay-as-you-go pricing across the entire 1,048,576-token context window. There are no price tiers or rate increases as context lengths scale up toward the one-million-token limit. Direct access on Moonshot's platform requires a top-up of at least $1, and top-up totals determine your rate-limit tier.

Claude Opus 5.5 lists at $4 per 1M input tokens and $20 per 1M output tokens, exactly double the standard rates of Kimi K3.1 Pro. Claude Sonnet 5.5 matches Kimi K3.1 Pro at $2 per 1M input tokens and $10 per 1M output tokens. Anthropic bills the full 1M-token context window at standard rates for both models, with cache hits at $0.20 for Opus 5.5 and $0.10 for Sonnet 5.5, and cache writes at $5 or $8 for Opus 5.5 and $2.50 or $4 for Sonnet 5.5.

GPT-6.1 Sol lists at $2 per 1M input tokens, $0.10 per 1M cached tokens, and $10 per 1M output tokens for requests up to 272K input tokens. Above 272K input tokens, GPT-6.1 Sol shifts to $4 per 1M input tokens, $0.20 per 1M cached tokens, and $15 per 1M output tokens. Gemini 4 Argon lists at $2 per 1M input tokens, $10 per 1M output tokens, and $0.10 per 1M cached tokens until November 30, 2026, after which it lists at $4 per 1M input tokens, $20 per 1M output tokens, and $0.20 per 1M cached tokens starting December 1, 2026.

Workload A shows the cost of 1M input tokens and 250K output tokens at list prices. On this workload, Kimi K3.1 Pro, Claude Sonnet 5.5, GPT-6.1 Sol, and Gemini 4 Argon (until November 30) all total $4.50. Kimi K3 totals $6.75, while Claude Opus 5.5 and Kimi K3.1 Pro Highspeed both cost $9.00.


*Cost of 1M input plus 250K output tokens at official standard list prices, checked October 24, 2026. Price is not capability: test on your own prompts.*

Workload B measures a single long-context request with 500K input tokens and 20K output tokens. For this request, Kimi K3.1 Pro, Claude Sonnet 5.5, and Gemini 4 Argon (until November 30) each cost $1.20. In contrast, GPT-6.1 Sol costs $2.30 because its higher rate tier applies above 272K tokens, while Claude Opus 5.5 and Kimi K3.1 Pro Highspeed each cost $2.40.


*Cost of one request with 500K input plus 20K output tokens. Kimi K3.1 Pro, Claude and Gemini 4 Argon keep one rate across the window; GPT-6.1 Sol bills requests above 272K input tokens at its long-context rate.*

Caching economics introduce a specific divergence for repetitive workloads. Cached input on K3.1 Pro costs $0.20 per 1M tokens, which is 10% of its base input price. Claude Sonnet 5.5 and GPT-6.1 Sol charge $0.10 per 1M cached tokens, cutting cache hit costs in half relative to Moonshot.

That difference changes the ranking for agent loops that reuse long prefixes. If a team runs deep multi-turn sessions where the same system instructions or documents are cached again and again, Sonnet 5.5 and GPT-6.1 Sol charge less per cached token than Kimi K3.1 Pro, even though all three share the same $2 base input price.

Kimi K3.1 Pro Highspeed is the same model served at 2 to 3 times the generation speed for $4 per 1M input tokens and $20 per 1M output tokens. That doubling in token pricing directly matches the price tag of Claude Opus 5.5. On a 1M input and 250K output workload, Highspeed reaches $9.00 compared to $4.50 on the standard tier.

This pricing premium pays off specifically when latency dictates system success. Interactive developer tools, synchronous code completion, and customer-facing interfaces benefit from cutting generation times by half or more. Moonshot keeps the cache hit price identical across both tiers at $0.20 per 1M tokens, but cache writes cost $4 per 1M tokens on Highspeed compared to $2 on K3.1 Pro.

For asynchronous workflows like offline evaluation, data extraction, or night-time batch processing, the standard Kimi K3.1 Pro model provides identical quality at half the price. Engineering teams should measure whether user drop-off or task timeouts justify paying double for token throughput before setting Highspeed as the default endpoint.

Moonshot's K3.1 Pro quickstart defines several fixed parameters that must be omitted from API requests. The API locks temperature to 1.0, top_p to 0.95, n to 1, presence_penalty to 0, and frequency_penalty to 0. Moonshot asks callers to leave them out of every request.

Thinking is always turned on in K3.1 Pro and cannot be disabled. The reasoning_effort parameter accepts low, high, and max, with max set as the default. Moonshot suggests setting reasoning_effort to low to cut reasoning time.

Handling multi-turn conversations and tool calling requires strict message preservation. In multi-turn and tool-call flows, applications must send the complete assistant message back unchanged in subsequent turns.

Output token caps are flexible across the full million-token window. The max_completion_tokens parameter defaults to 131,072 and can be configured up to 1,048,576. For multimodal requests, images must be submitted as base64 data URLs or uploaded file IDs, as public image URLs are not accepted. Moonshot also notes that its built-in web search is being updated and is not recommended for production workloads for now.

Moonshot is phasing out its previous generation of models according to a published timeline. From October 31, 2026, Kimi K2 series models are no longer available to newly registered users on Moonshot's platform. Existing users are not affected by that change.

Moonshot plans to retire all K2 series models entirely by the end of December 2026. Moonshot recommends migrating K2 traffic to Kimi K3.1 Pro. Teams already on Kimi K3 also save a third by moving: $2 per 1M input tokens and $10 per 1M output tokens instead of $3 and $15.

Tokely lists Kimi K3.1 Pro at Moonshot's list price of $2 per 1M input tokens, $10 per 1M output tokens, and $0.20 per 1M cached tokens. Kimi K3.1 Pro Highspeed is also available at list price for $4 per 1M input tokens, $20 per 1M output tokens, and $0.20 per 1M cached tokens. The gateway provides access through an OpenAI-compatible interface, allowing developers to switch models by updating the model identifier.

A single Tokely account, API key, and credit balance connects to Kimi K3.1 Pro alongside Western flagship models. On Tokely, Claude Opus 5.5 lists at $3 per 1M input and $15 per 1M output tokens, Claude Sonnet 5.5 lists at $1.50 per 1M input and $7.50 per 1M output tokens, GPT-6.1 Sol lists at $0.80 per 1M input and $4 per 1M output tokens, and Gemini 4 Argon lists at $1.40 per 1M input and $7 per 1M output tokens. Tokely prices can change, and developers should check the Tokely models page for current rates.

Testing model variations side by side requires no custom client code. The Tokely Playground allows teams to run the same prompt against Kimi K3.1 Pro and competing flagship models simultaneously to evaluate speed, reasoning depth, and formatting. You can call Kimi K3.1 Pro on Tokely with the standard OpenAI Python SDK using the following snippet:


```
from openai import OpenAI
client = OpenAI(base_url="https://api.tokely.me/v1", api_key="YOUR_TOKELY_API_KEY")
reply = client.chat.completions.create(
    model="kimi-k3.1-pro",
    messages=[{"role": "user", "content": "Review this function for race conditions."}],
    max_tokens=4096,
)
print(reply.choices[0].message.content)
```
Prices, parameters and migration dates come from the developers' documentation, checked on October 24, 2026; the Highspeed cache write price comes from the Vercel AI Gateway model list. Review implementation details in the Kimi K3.1 Pro quickstart and examine current rates on the Kimi API pricing page. Compare these figures with Western flagship documentation via Claude pricing, OpenAI API pricing, and Gemini API pricing. For background on training data filtering and the open release schedule, read The Next Web on the release. For extended context cost evaluations, consult our earlier comparison Gemini 4 Argon Pricing vs Claude Sonnet 5.5 and GPT-6.1 Sol.

*Originally published at tokely.me.*

## Top comments (0)
