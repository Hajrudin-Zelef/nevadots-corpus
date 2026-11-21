---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-vs-carbon-vs-argon-2026-which-to-use-pandromeda
title: "Gemini 4 Hydrogen vs Carbon vs Argon [2026]: Which to Use | Pandromeda"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-30"]
keywords: ["google", "argon", "gemini", "carbon", "model", "hydrogen", "work", "different"]
source: https://pandromeda.com/ai/gemini-4-hydrogen-vs-carbon-vs-argon-which-should-you-use
source_anchor: ""
source_lines: [1, 111]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen vs. Carbon vs. Argon: Which Should You Use?

Argon, Carbon and Hydrogen all carry the Gemini 4 name, but they solve different problems at very different prices. Here's the real difference, in Google's own numbers.

Google now sells three different models under the Gemini 4 name, and they are not interchangeable. **Argon** (shipped September 30, 2026) is the coding- and cyber-focused flagship. **Carbon** (generally available November 10, 2026) is Google's most capable model, built for the hardest multi-step reasoning and research work. **Hydrogen** (launched November 12, 2026) is the fast, cheap model that now runs Google Search's AI Mode by default and handles everyday agentic tasks. If you just want an answer: use Hydrogen for speed and cost, Carbon when a task genuinely needs maximum intelligence, and Argon for long, security- or code-heavy work where its cyber-defense tooling matters.

**The 10-second version**

- **Gemini 4 Hydrogen** — cheapest and fastest, 1M-token context, now the default model behind Google Search's AI Mode.
- **Gemini 4 Carbon** — Google's strongest model, same 1M-token context, priced like Argon, built for complex coding, science and agentic research.
- **Gemini 4 Argon** — the original Gemini 4 flagship, tuned for software engineering, enterprise knowledge work and cyber defense.
- All three share a 1-million-token context window and can output up to 1M tokens in long-decode mode, per Google's own documentation.

## What each Gemini 4 variant is actually for

Google has been unusually explicit about which model to reach for. In its own developer materials, Argon is positioned for "real-world coding, enterprise knowledge work, and cyber defense," Carbon is described as the model to use when a task "pushes the limits of model intelligence," and Hydrogen is called the company's "daily driver for agentic workflows" — the model meant to be used constantly, not saved for special occasions.

That's a different structure than the old Pro/Flash split. Argon and Carbon are both frontier-tier models with identical headline pricing; the difference between them is reasoning depth and recommended use case, not raw cost. Hydrogen sits underneath both as the economical, high-throughput option — and, notably, it's the one Google chose to put in front of billions of Search users for free. For broader context on how the three fit together as a family, see our explainer on what Gemini 4 is and how its variants relate.

## Gemini 4 Argon: the original flagship

Argon was Google's first Gemini 4 release, announced on September 30, 2026 as what Google called its "frontier model for real-world coding, enterprise knowledge work, and cyber defense." It rolled out first, in a less-restricted form, to trusted cyber defenders through Google's Fairwind Program, before reaching paid API customers and Google AI Ultra subscribers.

Google reported Argon state-of-the-art results on DeepSWE v1.1 (77.9%) and the top spot on Zapier's AutomationBench (51.3%), along with leading marks on legal, finance and video-understanding benchmarks — all company-reported figures rather than independently audited scores. Argon's output ceiling was also a headline feature: Google raised the per-response output limit to up to 1 million tokens, a large jump from the 64K cap on earlier Gemini models.

On pricing, Argon launched at an introductory $2 per million input tokens and $10 per million output tokens, with cached input priced 95% below the standard input rate. Our deeper rundown of what Argon changed is in this explainer on Google's new flagship model.

## Gemini 4 Carbon: Google's strongest model

Carbon reached general availability on November 10, 2026, after a two-week early period limited to cybersecurity defenders. Google's own description is direct: Carbon is the company's "strongest model," meant for work that "pushes the limits of model intelligence" — complex multi-step coding and debugging, and open-ended scientific research. Google still recommends Argon for most day-to-day software engineering and knowledge work, reserving Carbon for the harder tail of tasks where it uses more tokens to think longer.

Google's reported benchmark results for Carbon include the #1 spot on CursorBench 4.0, Perplexity's WANDR benchmark and Zapier's AutomationBench (at more than double the cost-efficiency of the next-best external model), plus a #2 finish on the Multimodal Knowledge Agent benchmark. On agentic honesty testing, Google reported Carbon passing 92% of a 375-task benchmark versus 70% for Argon, and a sharply lower automated-cyberattack success rate (2.17% versus Argon's 9.86%) on Gray Swan's test suite.

Carbon is available through the Gemini API, Google AI Studio and Gemini for business for paying developers and enterprises, and is rolling out to Google AI Ultra subscribers in the Gemini app, AI Mode in Search, and Google Antigravity (including a multi-agent "/swarm" preview). Pricing mirrors Argon's introductory structure: $2 per million input tokens and $10 per million output tokens through November 30, 2026, rising to $4 and $20 after that. More detail is in our full writeup of what's confirmed so far about Carbon.

## Gemini 4 Hydrogen: the fast model now running Search

Hydrogen is the newest of the three, introduced by Google on November 12, 2026 (the post carries a November 13 update timestamp). Google calls it its "daily driver for agentic workflows" — built for rapid execution across everyday coding and agentic tasks, rather than the deepest reasoning. It's also designed to work alongside the two frontier models: in Google Antigravity's Orchestrator, Carbon can hand routine steps to Hydrogen to preserve most of Carbon's quality while using noticeably fewer tokens.

The headline move is in Search. Google's own Search team announced the same day that Hydrogen is "now available globally in Google Search for all users," calling it the biggest model upgrade to Search to date. Google says it improves handling of multi-part questions, typos and highly specific queries, broadens query fan-out across subtopics, and surfaces more links and structured visual answers in AI Mode. In the Gemini app, Google separately labels Hydrogen for "Everyday help," next to Argon for "Code & analysis" and Carbon for "Most complex work."

On speed, Google's developer post claims Hydrogen completes tasks 2.5 times faster than Gemini 3.8 Flash at its default high-thinking setting, with early partner testing on data-heavy workflows showing 2x–2.8x faster runs and 12%–23% lower token usage versus 3.8 Flash. Pricing is markedly cheaper than Argon or Carbon: $1.50 per million input tokens and $7.50 per million output tokens, with cached input at $0.075 per million — and no scheduled price increase, unlike the two frontier models. Hydrogen is also the only one of the three available on Google's free API tier. For the full rundown of what changed in Search, see our coverage of Hydrogen's rollout.

## Gemini 4 Hydrogen vs. Carbon vs. Argon: specs and pricing

All three figures below come directly from Google's own Gemini API pricing page and model reference, current as of this writing. Pricing for Argon and Carbon is introductory and scheduled to roughly double on December 1, 2026; Hydrogen's pricing carries no such end date in Google's documentation.

| Model | Launch date | Input (per 1M tokens) | Output (per 1M tokens) | Context window | Max output | Best for | 
|---|---|---|---|---|---|---|
| Gemini 4 Argon | Sept 30, 2026 | $2.00 → $4.00 (Dec 1) | $10.00 → $20.00 (Dec 1) | 1M tokens | Up to 1M tokens | Coding, enterprise knowledge work, cyber defense | 
| Gemini 4 Carbon | Nov 10, 2026 | $2.00 → $4.00 (Dec 1) | $10.00 → $20.00 (Dec 1) | 1M tokens | Up to 1M tokens | Hardest multi-step coding, open-ended research | 
| Gemini 4 Hydrogen | Nov 12, 2026 | $1.50 (flat) | $7.50 (flat) | 1M tokens | Up to 1M tokens | Everyday tasks, agentic execution, Google Search | 

A detail worth flagging: Google's pricing page lists cached-input tokens at $0.10 (rising to $0.20) for Argon and Carbon, and $0.075 for Hydrogen — in each case roughly 95% off that model's standard input rate. Google's model reference also shows an unusual output notation, "64K / 1M tokens," for all three models, which in Google's own Argon announcement corresponds to a standard 64K output cap that can be extended to a 1M-token "long-decode" ceiling for tasks that need it.

## Speed vs. capability vs. cost: the real tradeoff

Strip away the marketing and the three models reduce to a fairly simple tradeoff curve. Carbon spends more tokens thinking, in exchange for the best results on the hardest benchmarks Google publishes — multi-step coding, scientific reasoning, and the toughest agentic honesty and financial-analysis tests. Argon sits just behind it on raw capability but is the one Google still recommends for most software engineering and enterprise knowledge work, likely because it's a known quantity with a longer track record and identical pricing to Carbon. Hydrogen trades some of that ceiling for speed and cost: it's roughly 25% cheaper on input and 25% cheaper on output than the frontier pair, and Google's own partner-reported numbers put it at 2x to 2.8x faster on comparable workloads.

That math is exactly why Google picked Hydrogen, not Carbon or Argon, to power Search by default. A search query needs a response in well under a second at effectively zero marginal cost to Google across billions of queries a day; a frontier reasoning model's extra accuracy wouldn't be worth the latency or compute bill at that scale. The same logic applies inside Antigravity's Orchestrator, where Carbon is used to plan and Hydrogen is used to execute the routine steps — described by Google as preserving most of Carbon's task success while cutting both time and token usage.

## Which Gemini 4 model should you actually use?

For most people typing into Google Search or the Gemini app's default chat, the answer is already made for you: Hydrogen is doing the work, and there's nothing to configure. For developers and teams choosing a model through the API, the decision comes down to what the task tolerates:

- **Pick Hydrogen** for high-volume, latency-sensitive, or cost-sensitive work — chat assistants, everyday coding help, agent execution steps, anything run at scale where a 25%+ cost difference compounds quickly.
- **Pick Argon** for software engineering and enterprise knowledge work (legal, financial analysis) where you want a frontier-tier model with mature cyber-defense tooling and a five-week-longer track record than Carbon.
- **Pick Carbon** when the task is genuinely at the edge of what the model family can do — open-ended research, the hardest multi-step debugging, or agentic work where Google's own benchmarks show the largest gap over Argon.

Budget matters too: because Argon and Carbon share pricing, there's no cost penalty for defaulting to Carbon on tasks where you're unsure — the only cost is that it may use more tokens per response to do its extra reasoning. Hydrogen is the one case where the price difference is structural and durable, since Google hasn't attached an end date to its lower rate the way it has for the other two.

## What happens next

Two dates are already on the calendar. Argon and Carbon's introductory pricing is set to roughly double on December 1, 2026, which will close most of the cost gap between "frontier" and "fast" Gemini 4 tiers even further in Hydrogen's favor unless Google extends the discount. And Google has already signaled Carbon and Hydrogen will keep working together rather than competing head-on: Antigravity's Orchestrator pattern of Carbon-plans/Hydrogen-executes looks like the template for how Google expects most agentic products to use the family going forward, rather than developers picking a single model for an entire workflow. Expect Google to keep tuning that split, and possibly introduce further variants, as it learns how the three get used in practice. If a cross-vendor view is more useful than an intra-family one, our comparison of Gemini 4 Argon against GPT-6 Astra and Claude Opus 5.5 covers how Google's flagship stacks up against rivals.

## Frequently asked questions

### What's the difference between Gemini 4 Argon, Carbon and Hydrogen?

Argon (launched September 30, 2026) is Google's original Gemini 4 flagship for coding, enterprise knowledge work and cyber defense. Carbon (November 10, 2026) is Google's strongest model, for the hardest multi-step reasoning and research tasks. Hydrogen (November 12, 2026) is a faster, cheaper model built for everyday agentic work, and it's the model now powering Google Search's AI Mode by default.

### Which Gemini 4 model is cheapest?

Gemini 4 Hydrogen, at $1.50 per million input tokens and $7.50 per million output tokens on Google's standard tier, versus $2/$10 (rising to $4/$20 on December 1, 2026) for both Argon and Carbon, per Google's Gemini API pricing page.

### Which Gemini 4 model powers Google Search now?

Gemini 4 Hydrogen. Google's Search team announced on November 12, 2026 that Hydrogen is available globally in Google Search's AI Mode for all users, calling it the biggest model upgrade to Search to date.

### Is Gemini 4 Carbon better than Gemini 4 Argon?

Google positions Carbon as its strongest model, with better reported results on several benchmarks including agentic honesty and cybersecurity testing, but it still recommends Argon for most everyday software engineering and knowledge work. The two share identical introductory pricing.

### What are the context window and output limits across Gemini 4 Argon, Carbon and Hydrogen?

According to Google's own model reference, all three share a 1-million-token context window and can output up to 1 million tokens in long-decode mode, with a standard 64K-token output cap otherwise.

### Is Gemini 4 Hydrogen the same as the old Gemini Flash models?

Google has not described Hydrogen as a renamed Flash model; it calls Hydrogen its 'daily driver for agentic workflows' and compares its speed directly against the prior Gemini 3.8 Flash model, claiming roughly 2x to 2.8x faster task completion.

## Sources

- Google: Gemini 4 Argon — our next era of frontier intelligenceblog.google
- Google: Gemini 4 Carbon is now availableblog.google
- Google: Introducing Gemini 4 Hydrogenblog.google
- Google Search Blog: Gemini 4 Hydrogen Search upgradeblog.google
- Google AI Studio: Gemini API pricingaistudio.google.com
- Google AI Studio: Gemini models referenceaistudio.google.com

Theo Park runs the AI desk at Pandromeda. He follows model launches from the frontier labs and the open-weight community, tracks the assistants and developer tools built on them, and explains what each release changes on pricing, capability and safety. His reporting leans on primary sources: model cards, technical reports, API documentation and the companies' own announcements.
