---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-pricing-membership-plans-api-costs
title: "Kimi K3.1 Pro Pricing | Membership Plans & API Costs"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-10-24"]
keywords: ["included", "kimi", "mtok", "membership", "pricing", "highspeed", "code", "agent"]
source: https://www.kimi.ai/resources/kimi-k3-1-pro-pricing
source_anchor: ""
source_lines: [1, 85]
section: "Kimi"
---
Kimi K3.1 Pro is Moonshot AI’s most capable 3T-class open-weight model to date, with a 1M-token context window for coding, 3D creation, financial research, and agent workflows. Below, we compare Kimi membership plans, cross-product benefits, and standard and Highspeed API pricing.

Kimi K3.1 Pro and Kimi K3.1 Pro Highspeed are now available on Kimi on the web, Kimi Work, Kimi Code, and Kimi API. You can use Kimi’s own products through a membership plan or integrate the models into your applications with pay-as-you-go API access.

## Kimi K3.1 Pro membership plans and usage tiers

Compare personal membership prices, model access, and workspace benefits below. Plus adds Kimi Code access; Pro unlocks K3.1 Pro Highspeed across Kimi on the web, Kimi Work, and Kimi Code; Max and Ultra add million-token extra-long chat capacity and higher usage allowances.

| Plan / benefit | Free | Plus | Pro | Max | Ultra | 
|---|---|---|---|---|---|
| Annual billing: effective monthly | $0 | $15 / month | $31 / month | $79 / month | $159 / month | 
| K3.1 Pro — Kimi on the web and Kimi Work | Limited usage | Included | Included | Included | Included | 
| K3.1 Pro — Kimi Code | / | Included | Included | Included | Included | 
| K3.1 Pro Highspeed — Kimi on the web, Kimi Work, and Kimi Code | / | / | Included | Included | Included | 
| Agent credits | Basic | More | 2× | 5× | 10× | 
| K3 Extra-long chat capacity: up to 1M tokens | / | / | / | Included | Included | 
| Agent concurrent tasks | 1 | 2 | 2 | 4 | 4 | 
| Agent priority generation queue | / | 4× speed | 4× speed | 4× speed | 4× speed | 
| Scheduled tasks | 2 | 10 | 15 | 20 | 25 | 
| Widget tasks | 2 | 10 | 15 | 20 | 25 | 
| Projects | 2 | 20 | 20 | 100 | 100 | 
| Project storage | 500MB | 20GB | 20GB | 50GB | 50GB | 
| Swarm: concurrent subtasks | / | 2 | 4 | 8 | 8 | 
| Dream Memory | / | Included | Included | Included | Included | 
| Self-evolving Skill | / | Included | Included | Included | Included | 
| Goal mode | / | / | Included | Included | Included | 
| Kimi Claw: cloud, Android local, and desktop | / | / | Included | Included | Included | 
| Publish a website with a database | / | Included | Included | Included | Included | 

Paid memberships also support Office file processing, deep research, website deployment, and specialist financial and economic databases through selected plugins.

All prices are in USD. Annual subscriptions are billed at the yearly totals shown, with the effective monthly rates provided for comparison. See the official membership page for current plan details.

## Kimi K3.1 Pro API pricing

The Kimi API uses pay-as-you-go billing. K3.1 Pro uses flat per-token pricing across its context window, without context-length pricing tiers.

| Model | Base tokens |  | Prompt caching |  |  | 
|---|---|---|---|---|---|
| Name | Input | Output | Cached Input | 5m writes | 1h writes | 
| kimi-k3.1-pro | $2.00 / MTok | $10.00 / MTok | $0.20 / MTok | $2.00 / MTok | $4.00 / MTok | 
| kimi-k3.1-pro-highspeed | $4.00 / MTok | $20.00 / MTok | $0.20 / MTok | $4.00 / MTok | $8.00 / MTok | 

*All rates below are in USD per 1M tokens (1,000,000 tokens), excluding applicable taxes. Both models support a 1,048,576-token context window. Check official Kimi API pricing for more details. Rates checked on October 24, 2026. Taxes depend on your billing jurisdiction.

### Kimi K3.1 Pro vs. Highspeed: which should you choose?

- **For lower token costs:** Choose kimi-k3.1-pro when you want lower token costs for code generation, document analysis, financial research, or agent tasks where response speed is less critical.
- **For faster output:** Choose kimi-k3.1-pro-highspeed when faster output matters, such as interactive coding, live assistants, and workflows where people are waiting for the next response.

Highspeed doubles the listed regular-input, output, and cache-write rates. Its cached-input rate stays at $0.20 per million tokens, the same as the standard model. A request with substantial cached context therefore needs a separate cost calculation rather than simply doubling the standard-model bill.

For comparison, Kimi K3 is currently listed at $3.00 per million regular input tokens, **USD 15.00 per million** output tokens, and **USD 0.30 per million** cached input tokens. Standard K3.1 Pro is one-third lower on each of those rates. Your actual bill still depends on token use and cache behavior.

## How Kimi K3.1 Pro API billing works

API input includes prompts, system instructions, conversation history, documents, and tool definitions. The bill separates cache hits, new cache writes, the uncached remainder, and output tokens.

### Regular input, cache writes, and cache hits

**- Cache writes and TTL**

Repeated request prefixes can be cached automatically. Newly cached tokens are charged at the selected cache-write rate. The default TTL is 5 minutes, with a 1-hour option for longer gaps between requests.

**- Cache hits and prefix matching**

Reusing an unchanged prefix is billed at the cache-hit rate and refreshes its existing TTL without another write fee. Similar wording alone does not guarantee a hit: changes early in a prefix prevent reuse after that point.

**- Input token accounting**

For Chat Completions and Responses, cache hits, cache writes, and uncached tokens are mutually exclusive and add up to total input. Tokens that neither hit nor write to the cache use the regular input rate. Each input token belongs to one category, so cache-write tokens should not also be charged as regular input.

### Output tokens and reasoning effort

Output costs **USD 10.00 per million tokens** on standard K3.1 Pro, or **USD 20.00** on Highspeed. Use reported output usage rather than final-answer length alone. K3.1 Pro always reasons; `reasoning_effort` supports `low`, `high`, and `max`, with `max` as the default. Test lower effort on simpler tasks to balance quality, latency, and token use.

**Estimated cost** = (uncached tokens × regular-input rate + cache-hit tokens × cached-input rate + 5m-write tokens × 5m-write rate + 1h-write tokens × 1h-write rate + output tokens × output rate) ÷ 1,000,000. See the context-caching guide for usage fields and TTL configuration.

## Conclusion

Kimi K3.1 Pro brings advanced coding, 3D creation, financial research, and agentic workflows together in one model. Its 1M-token context window handles large codebases, lengthy documents, and complex projects with less fragmentation, while strong reasoning supports demanding, end-to-end work. Standard and Highspeed options let you prioritize cost efficiency or responsiveness without compromising capability. For developers, creators, and professionals ready to move from ideas to polished results, Kimi K3.1 Pro is a powerful choice for building, researching, and creating with confidence.

## FAQ

`reasoning_effort` to `low`, `high`, or `max`; `max` is the default. Measure how a lower setting affects token use and answer quality for your tasks.
