---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-ai-model-benchmarks-pricing-features
title: "Gemini 4 Hydrogen: AI Model Benchmarks, Pricing & Features"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hydrogen", "argon", "carbon", "gemini", "google", "model", "xhigh", "million"]
source: https://aiincontext.com/articles/google-s-gemini-4-hydrogen-beats-larger-argon-four-weeks-after-launch
source_anchor: ""
source_lines: [1, 67]
section: "Gemini_Google"
---
## Why it matters

- Gemini 4 Hydrogen scores 51 vs. Argon's 50 on the AA Intelligence Index at the default level, and 54 at xhigh.
- Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens, with cache hits at $0.075.
- Terminal-Bench 4.1 jumps from 18.2% (Gemini 3.8 Flash) to 57% (Hydrogen); hallucinations drop fivefold.
- Artificial Analysis measures Hydrogen at ~$3.16 per task at xhigh, about a third of Claude Sonnet 5.5 (max).
- Hydrogen supports a one-million-token input window and one-million-token output, with four thinking levels.

Google has released Gemini 4 Hydrogen, a fast general-purpose AI model that outscores the larger Gemini 4 Argon on the Artificial Analysis Intelligence Index at the default thinking level — 51 versus 50 — and reaches 54 at its highest "xhigh" tier. The release lands less than four weeks after Argon's debut, effectively rendering that frontier model obsolete for everyday work.

Hydrogen is available now through the Gemini API in AI Studio and Android Studio, the Gemini app, Google Search, Google Antigravity, and Gemini for Business. Paying Pro and Ultra subscribers get access through Antigravity, Google's coding tool.

## Why does Hydrogen replace Argon so quickly?

Google designed Hydrogen for everyday coding and agentic workflows, not the deep, long-horizon reasoning that defines the frontier models Gemini 4 Carbon and Gemini 4 Argon. It is the successor to the Flash series: Google positions it as a replacement for Gemini 3.8 Flash, but the benchmarks suggest a bigger leap.

The pricing inversion is the striking part. Hydrogen costs $1.50 per million input tokens and $7.50 per million output tokens, with cache hits at $0.075. According to Google's own numbers, it ends up both more capable and cheaper than the larger Argon.

The timing compounds the confusion. When Google announced its new frontier model Carbon just two days before the Hydrogen release, the company recommended sticking with Argon for daily coding work. Hydrogen makes that advice moot.

The model supports a one-million-token context window for input and can output one million tokens. It offers four thinking levels — low, medium, high (the default), and a new xhigh tier — while Argon offers only medium and high.

## How big is the jump over Gemini 3.8 Flash?

At the default thinking level, Google says Hydrogen finishes tasks 2.5 times faster than its predecessor. On Terminal-Bench 4.1, the score jumps from 18.2 percent for Flash to 57 percent for Hydrogen, measured on a cheating-adjusted basis. Google reports hallucinations dropped by a factor of five, and the model recovers better when a tool call fails or a search returns nothing.

On agentic benchmarks, Hydrogen ranks fifth on the Agent Arena leaderboard for open-ended agent tasks such as research, data analysis, and code navigation. It takes first place on the Vals Tax Agent Bench with 52.3 percent and ties for first on the Vals Legal Research Bench with 55.8 percent.

The full comparison against Carbon and Argon, per Google's published table:

- **General abilities** : AA Index v5.0 — Hydrogen 54 (xhigh, 5.1), Carbon 59, Argon 50. Vals Index v2.2 — Carbon 70.4%, Argon 69.2%.
- **Agentic coding** : SWE-sweep — Hydrogen 13.5%, Carbon 9.6%, Argon 4.3%. FrontierSWE 2 — Carbon 61.2%, Argon 55.0%. Terminal Bench 4.0 (AA) — Carbon 65.2%, Argon 57.1%.
- **Knowledge work** : Agents' Last Exam v1.1 — Hydrogen 71.6%, Carbon 77.4%, Argon 69.0%. AutomationBench — Hydrogen 76.1%. AutomationBench (Zapier) — Carbon 53.9%, Argon 51.3%. Vending-Bench 2 account balance — Hydrogen $14,549, Carbon $16,484, Argon $13,718.
- **Computer use** : OSWorld-2.1 offline subset — Hydrogen 77.6%, Carbon 81.5%, Argon 74.7%.
- **Multimodal understanding** : MKA Climb WSR accuracy — Hydrogen 53.4%. Chartography — Hydrogen 66.6%, Carbon 72.5%, Argon 71.6%. Blueprint Bench 2 similarity — Carbon 58.6%, Argon 54.4%.
- **Long context** : GraphWalks (256k to 1M, BFS) — Carbon 96.2%, Argon 84.2%.
- **Science and mathematics** : Terminal-Bench-Science 0.1 — Hydrogen 51.4%. RiemannBench — Carbon 80.0%, Argon 76.0%. Terminal-Bench-Science 0.1 (AA) — Carbon 64.8%, Argon 52.9%.
- **Reasoning** : HLE-Diamond — Hydrogen 58.8%.
- **Cybersecurity** : CWE-bench — Carbon 71.0%, Argon 68.0%. ExploitGym — Carbon 54.8%, Argon 42.0%. SEC-bench Pro — Carbon 88.2%, Argon 83.4%.

For multimodal knowledge work like summarizing large PDFs, spreadsheets, or videos, Hydrogen supports up to one million output tokens on top of its one-million-token input window. It scores 53.4 percent on Google's new Multimodal Knowledge Agent Benchmark and 71.6 percent on Agents' Last Exam v1.1.

## What changes in Google's apps?

In the Gemini app, Google says the model adheres more closely to tone, format, and instructions over longer conversations, so drafts, summaries, and documents are more often usable on the first try. Hydrogen can pull in context from Google apps like Gmail and Drive, plus third-party apps including Airtable and Dropbox.

Google calls Hydrogen a "significant upgrade" for Search. The model handles queries better even when they contain typos, multi-part questions, or very specific requests, according to Google. Results are more visual, easier to scan, and an improved query fan-out technique (splitting a query into multiple parallel searches) retrieves a wider range of relevant links.

In Antigravity, a new orchestrator switches dynamically between Carbon and Hydrogen within a session instead of committing to one model. On Terminal-Bench 4.0, Google says the orchestrator nearly matches Carbon's success rates while using 24 percent fewer resources and cutting processing time by 19 percent.

## What does the independent evaluation say?

An independent evaluation by Artificial Analysis puts Hydrogen roughly on par with Claude Sonnet 5.5 at its highest thinking level. At the default "high" level, the score drops to 51, slightly behind GPT-6.1 Sol (xhigh, 52) and Claude Opus 5.5 (high, 52).

On the AA-Omniscience benchmark, Hydrogen makes things up only 8 percent of the time, Artificial Analysis reports. It also beats other Gemini models, including Carbon, on presentation quality in AA-Briefcase, which evaluates agentic knowledge work.

Terminal work is the weak spot. Hydrogen scores 54 percent on Terminal-Bench 4.0 and 50 percent on Terminal-Bench-Science 0.1, while Claude Sonnet 5.5 (max) reaches 64 and 53 percent respectively and GPT-6.1 Sol (max) scores 56 and 58 percent.

## Is Hydrogen the price-performance leader?

On price-performance, Artificial Analysis puts Hydrogen at about $3.16 per task at xhigh — roughly a third of what Claude Sonnet 5.5 (max) costs at nearly the same intelligence level, but about 2.5 times as much as GPT-6.1 Sol (max).

Costs fall steeply at lower tiers: $1.87 at "high," $0.80 at "medium," and $0.29 at "low." Intelligence falls with them. Medium reaches only 41 on the index, low just 27. Artificial Analysis notes that xhigh consumes nearly twice as many output tokens as high, which explains the price jump. At the high, medium, and low tiers, the model sits on the Pareto frontier (the set of options where no other model is both cheaper and smarter at that price point).

The release signals that Google's mid-tier cadence now outpaces the shelf life of its own frontier models: Argon, launched four weeks earlier and recommended for daily coding two days ago, no longer holds a clear advantage. Carbon remains ahead on long-context, cybersecurity, and science benchmarks, but the gap between Google's cheapest and most expensive tiers has narrowed to a question of a few index points.

Original: artificialanalysis.ai
