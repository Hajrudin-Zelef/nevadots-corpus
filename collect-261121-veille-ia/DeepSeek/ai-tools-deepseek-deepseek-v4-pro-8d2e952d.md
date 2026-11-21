---
id: collect-261121-veille-ia/deepseek/ai-tools-deepseek-deepseek-v4-pro-8d2e952d
title: "Ai tools deepseek deepseek v4 pro"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-08-13"]
keywords: ["deepseek", "pricing", "model", "peak", "rate", "deepseek-v4-pro", "input", "august"]
source: ""
source_anchor: ""
source_lines: [1, 48]
section: "DeepSeek"
---
DeepSeek V4 Pro: GA Release, New Pricing, and What Actually Changed
DeepSeek's flagship model grew up and got more expensive in the same week. On August 13, 2026, DeepSeek-V4-Pro left preview and reached general availability, per the official GA announcement; the pricing page now lists the build as DeepSeek-V4-Pro-0813. Three days later, at 16:00 UTC on August 16, the flat per-token rates it had carried since spring gave way to peak-hour billing, verified live against DeepSeek's API pricing page. Depending on when your tokens run, output now costs 2.3x to 4.6x what it did the week before.
This breakdown covers what actually shipped at GA, the new pricing in full, which benchmark claims are verified and which are not, what the newly open-sourced DeepSeek Harness is, and who needs to act on any of it. Every price here is dated, because this pricing story has already changed three times in four months.
What DeepSeek Shipped on August 13, 2026
V4 Pro is the larger half of DeepSeek's V4 family: a 1.6-trillion-parameter open-weight mixture-of-experts model (a design that stores many specialist sub-networks but activates only a few at a time, which is why just 49 billion parameters work on each token) and a 1M-token context window, MIT-licensed, per the official Hugging Face model card. The smaller V4-Flash left preview on July 31; Pro followed two weeks later. Our V4 architecture breakdown covers the hybrid attention design both models share.
The GA release is an upgrade in place. The API model ID, deepseek-v4-pro, did not change, so any code already calling it was moved to the 0813 build automatically, as Digital Applied's release analysis confirmed. Three things are new: agent-focused workflow improvements, adjustable reasoning effort, meaning how much hidden deliberation the model spends before answering (low, high, and max, on both Pro and Flash, with the default level not stated on either surface), and native support for OpenAI's Responses API format, so tools built for OpenAI's newer API surface, including Codex-style coding agents, can point at DeepSeek without a rewrite. The open weights, roughly 893 GB across 67 safetensors files, shipped on Hugging Face the same day.
The New Pricing: Peak Hours Change the Math
A quick orientation if API billing is new to you. Usage is metered in tokens, small chunks of text a few characters long; a million tokens is roughly 750,000 English words. You pay one rate for input (everything you send the model) and a higher rate for output (everything it writes back). And when part of your input is something the provider has served recently, such as a repeated system prompt, it counts as a cache hit and bills at a steep discount; fresh input is a cache miss at the full rate.
Effective August 16, 2026 at 16:00 UTC, DeepSeek bills by time of day. Peak hours are 01:00 - 04:00 and 06:00 - 10:00 UTC; every other hour is off-peak and billed at 50 percent of the peak rate, per the official pricing page, verified the day the change took effect. That is seven peak hours and seventeen off-peak hours per day.
| Model and window | Input (cache miss) | Input (cache hit) | Output | 
|---|---|---|---|
| deepseek-v4-pro, peak (from Aug 16, 2026, 16:00 UTC) | $1.32 / 1M | $0.044 / 1M | $3.96 / 1M | 
| deepseek-v4-pro, off-peak (from Aug 16, 2026, 16:00 UTC) | $0.66 / 1M | $0.022 / 1M | $1.98 / 1M | 
| deepseek-v4-pro, flat rate until Aug 16, 2026, 16:00 UTC | $0.435 / 1M | $0.003625 / 1M | $0.87 / 1M | 
| deepseek-v4-flash, peak (from Aug 16, 2026, 16:00 UTC) | $0.44 / 1M | $0.014 / 1M | $1.32 / 1M | 
| deepseek-v4-flash, off-peak (from Aug 16, 2026, 16:00 UTC) | $0.22 / 1M | $0.007 / 1M | $0.66 / 1M | 
| deepseek-v4-flash, flat rate until Aug 16, 2026, 16:00 UTC | $0.14 / 1M | $0.0028 / 1M | $0.28 / 1M | 
How big an increase this is depends on which baseline you pick, and the choice of baseline is where most of the coverage goes sideways. Against the $0.435 / $0.87 rates customers were actually paying, peak output is a 4.6x jump and off-peak is 2.3x (computed from the two published rate cards). Against the $1.74 / $3.48 standard list price that DeepSeek published in April but never got around to charging, peak output is only 14 percent higher and off-peak is 43 percent lower. Headlines saying prices "quadrupled" are using the first baseline at peak; anyone calling it a modest adjustment is quietly using the second. The first is the honest one, because it describes real bills.
Why charge by the hour? DeepSeek hasn't said. The context worth knowing: peak-demand congestion on DeepSeek's API, surfacing as 503 errors and latency spikes during busy windows, has been a documented pattern through 2026 (our DeepSeek API guide covers the troubleshooting record). Time-of-day pricing pushes discretionary load out of the busiest windows, which is what a capacity-constrained provider would want. That reading is inference from the public record, not a stated motive.
How Much Does DeepSeek V4 Pro Cost Now?
For a workload you control the timing of, the answer is a range, and the range is the point. The same job costs exactly twice as much run entirely at peak as it does run entirely off-peak. Two levers matter more than the sticker price: what share of your traffic runs during the seven daily peak hours, and how much of your input is served from cache. Cache hits bill at roughly 97 percent below the cache-miss rate ($0.044 against $1.32 at peak, same ratio off-peak, computed from the published rate card), so agent scaffolding that resends the same system prompt every step is far cheaper than the raw numbers suggest.
The scheduling lever is unusual among major API vendors, and it cuts both ways. A latency-tolerant batch pipeline that runs entirely in off-peak hours pays $1.98 per million output tokens, still below most Western flagship output rates (GPT-5.6 Terra, for one, lists $12.00 per million output tokens even after its July cut, though the cheaper Luna tier sits lower). An interactive product serving users in Asian business hours, which overlap the UTC peak windows, pays the full $3.96 with no way to shift. Where your users live is now a pricing input. For the wider lineup context, see the complete DeepSeek pricing guide.
The Benchmark Claims, Read Honestly
DeepSeek published a GA benchmark table showing gains over the preview build. The uncomfortable part: the table exists only as an image on the announcement page, its code-agent rows come from DeepSeek's own tooling ("DeepSeek Harness, minimal mode," at maximum reasoning effort, per Digital Applied's analysis of the release), and as of publication no third party had reproduced any of it. Cherry Creek News checked benchable.ai's page for the model and found the third-party benchmark section empty.
The preview-era record still stands and is worth restating, with its labels attached. Vendor-reported scores from the April technical report, republished on the model card: 80.6 on SWE-bench Verified, 90.1 on GPQA Diamond, 67.9 on Terminal Bench 2.0. Independently, Artificial Analysis's April analysis put preview V4 Pro at 1554 on GDPval-AA, the leading open-weights score on that agentic work benchmark, while also measuring a 94 percent hallucination rate: when the model doesn't know an answer, it almost always produces one anyway. DeepSeek's own technical report, republished with the model card, concedes the model trails frontier systems like GPT-5.4 and Gemini 3.1 Pro by roughly three to six months. A vendor saying that about its own flagship is the most credible line in the whole release.
DeepSeek Harness: Real, Early, and Easy to Oversell
Alongside the GA release, DeepSeek published DeepSeek Harness (dsh), an open-source agent framework in TypeScript under an MIT license, built around the idea that every capability is a plugin. The repository's install instructions launch it with npx @deepseek-ai/dsh web, serving a local web UI (command from the project documentation as of August 16, 2026, not executed for this article). This is the tooling DeepSeek used to run its own GA code-agent benchmarks, which makes it a real artifact of how the company works, not just a marketing release.
Read the repository before reading the coverage. The project describes itself as a developer preview and states, in capitals, that compatibility-breaking changes are coming. Some press framed it as DeepSeek's answer to Claude Code; the repository itself makes no such comparison, and a plugin framework in developer preview is a different thing from a mature coding agent. If you experiment with it, pin versions and expect churn. That's not a criticism. It's what "developer preview" means, and DeepSeek said it plainly.
Who Should Care, and What to Watch
One scope note first: if you only use the free DeepSeek chat app, nothing here changes for you. The GA release and the repricing apply to the API, the metered service developers build on.
What to Watch Next
- Independent GA benchmarks. The vendor table is unreproduced. When benchable.ai, Artificial Analysis, or academic harnesses publish full 0813 runs, the real GA story arrives.
- Reseller repricing. OpenRouter, Together, Fireworks, and Bedrock set their own DeepSeek rates. None had been re-verified against the new scheme at publication; check the reseller's own price list before relying on a relayed rate.
- Whether peak windows move. The current windows track Asian daytime demand. If capacity improves, the schedule could soften; if not, expect the windows to be enforced in practice through throttling as well as price.
- Rival responses. This lands weeks after OpenAI's GPT-5.6 cuts and days after the Grok 4.6 and Gemini 3.7 Flash launches, both of which led with price. Whether DeepSeek raising prices amid a price war costs it the value crown is the question our cheapest frontier APIs ranking now has under review.
The Caveats, in One Place
Frequently Asked Questions
Go Deeper
Resources from across Tech Jacks Solutions
FREEAI Risk Management Template
Identify, assess, and mitigate AI vendor and deployment risk
EU AI Act Guide
Check your compliance obligations under the EU AI Act
AI Career Paths
Explore roles that work with these tools daily
What Is Agentic AI?
Understand the architecture behind autonomous AI agents
DeepSeek and the DeepSeek logo are trademarks of their respective owner. OpenAI, ChatGPT, GPT, and Codex are trademarks of OpenAI. Claude, Claude Code, and Fable are trademarks of Anthropic. This article is independent editorial content from Tech Jacks Solutions and is not affiliated with, endorsed by, or sponsored by DeepSeek or any vendor named.
