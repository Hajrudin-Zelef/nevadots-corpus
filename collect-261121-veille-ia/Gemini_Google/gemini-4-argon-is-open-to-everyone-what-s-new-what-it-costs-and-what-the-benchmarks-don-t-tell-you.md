---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-is-open-to-everyone-what-s-new-what-it-costs-and-what-the-benchmarks-don-t-tell-you
title: "Gemini 4 Argon Is Open to Everyone: What's New, What It Costs, and What the Benchmarks Don't Tell You"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "gemini", "tokens", "rate", "costs", "opus", "astra", "open"]
source: https://dev.to/neuraitool/gemini-4-argon-is-open-to-everyone-whats-new-what-it-costs-and-what-the-benchmarks-dont-tell-3pgn
source_anchor: ""
source_lines: [1, 54]
section: "Gemini_Google"
---
# Gemini 4 Argon Is Open to Everyone: What Changed, What It Costs, and What the Benchmarks Don't Tell You

Google announced Gemini 4 Argon on September 30, 2026 — then kept it behind the velvet rope for two weeks, accessible only to vetted cyber defenders in its Fairwind Program. On October 13, Argon went generally available to anyone on an eligible paid plan: the paid Gemini API, AI Studio, Antigravity, the Gemini app and AI Mode for Pro and Ultra subscribers, and four GitHub Copilot plans.

If you build on AI models, two things matter more than the spec sheet: the pricing has an expiration date, and the benchmark numbers were measured at a setting subscribers don't get by default.

Google's API pricing page lists introductory rates through November 30, 2026, and standard rates from December 1:

| Tier | Intro (until Nov 30) | Standard (from Dec 1) | 
|---|---|---|
| Standard input / output | $2.00 / $10.00 | $4.00 / $20.00 | 
| Cached input | $0.10 | $0.20 | 
| Batch and Flex in / out | $1.00 / $5.00 | $2.00 / $10.00 | 
| Priority in / out | $3.60 / $18.00 | $7.20 / $36.00 | 

Prices are per million tokens. Three details engineers keep missing:

1. 
**Output price includes thinking tokens.** Argon is a reasoning model; the tokens it spends thinking are billed at the output rate even though your users never see them. A reasoning-heavy workload pays far more than a chat demo suggests. Measure tokens per finished task, not per token.
2. 
**Cached input gets a 95% discount.** That's unusually generous and it matters most for agentic workflows that re-read the same context across steps. But caching is implicit only — you can't force a cache write.
3. 
**There is no free tier.** Free API keys and the free Gemini app plan stay on the Flash models.

At intro pricing Argon costs about the same as GPT-6.1 Sol, roughly half of Claude Opus 5.5. After December 1, its list price equals Opus 5.5's — so to justify it over Sonnet 5.5 or GPT-6.1 Sol afterward, it has to finish your task with half the tokens.

Google's own chart (18 tests) has Argon leading outright on 12 and tying on one: DeepSWE v1.1 at 77.9%, AutomationBench at 51.3%, Harvey Legal Agent at 19.6% (versus 3.8% for Opus 5.5), and a 0.7% prompt-injection success rate versus 8.5% for GPT-6 Astra.

The fine print: Google picked the tests, and the headline scores were measured at Argon's **High** reasoning setting. The subscriber default is **Medium** (Argon exposes Low, Medium, High). Independent testing from Artificial Analysis gives the balanced picture:

- Intelligence Index: 53 (High), a practical tie with GPT-6 Astra (53), about five points behind Claude Opus 5.5 (58) and Sonnet 5.5 (56).
- Hallucination rate on AA-Omniscience: **15%** — the lowest of any model scoring above 45 on the index, versus 51% for Astra.
- Accuracy on the same test: 50% versus 63% for Astra. The low hallucination rate comes partly from saying "I don't know" more often instead of guessing.
- AutomationBench-AA (independent run): 77.5%, ranked first.
- Text Arena: first at 1,525 points, though Arena marks it preliminary.

The pattern: Argon is genuinely strong on long documents, video, finance, and business automation; weaker on terminal-style agent work (Opus 5.5 leads Terminal-Bench 4.0 by nine points) and frontier software engineering (Astra leads FrontierSWE v2 by ten-plus).

Three practical takeaways:

1. 
**Prototype now, budget at December prices.** You have roughly seven weeks at the cheap rate. Wire it in October, run your evals in November, and decide before December 1 — wire it into a production invoice only at the $4/$20 rate.
2. 
**Measure thinking tokens as output.** Argon's reasoning is the product; your cost model has to count it. A task that "looks cheap" at $2/$10 per token can surprise you on the first bill.
3. 
**Use the hallucination profile where it wins.** A model that declines when unsure is a trust asset for policy summaries, customer-facing answers, and knowledge work — and a liability for hard knowledge questions where you'd rather have a guess.

Also note the migration traps if you're moving from Gemini 3.x: non-streaming REST requests cap at 32,768 tokens and use a continuation protocol, sampling parameters are accepted and discarded, and explicit caching returns an error. Run an eval set before changing the model string.

Gemini 4 Argon is the October release to evaluate in the frontier tier — not because Google's chart says it wins, but because seven weeks of half-price access is the cheapest you'll ever get to measure it honestly.

*This is a condensed technical explainer. The full breakdown — access routes per product, the complete pricing table, pros and cons, and FAQ — is on NeurAITool: Gemini 4 Argon Is Open to Everyone: What's New. For more tested AI tool coverage, browse the NeurAITool blog.*

## Top comments (0)
