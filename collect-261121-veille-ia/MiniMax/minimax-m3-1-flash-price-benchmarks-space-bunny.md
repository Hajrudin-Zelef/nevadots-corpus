---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-price-benchmarks-space-bunny
title: "MiniMax M3.1 Flash: Price, Benchmarks, Space Bunny"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["minimax", "openrouter", "flash", "tokens", "october", "space", "bunny", "price"]
source: https://cellcog.ai/blog/minimax-m3-1-flash/
source_anchor: ""
source_lines: [1, 105]
section: "MiniMax"
---
**MiniMax released M3.1 Flash on October 19, 2026, and confirmed it is the model that ran on OpenRouter as Space Bunny Alpha.** Its release post at 11:56 UTC lists an API price of $0.15 per million input tokens and $0.60 per million output tokens, a 1,000,000-token context, and weights on October 23. Five minutes later OpenRouter put it back as a free listing for 72 hours.

The benchmark numbers are MiniMax’s own and nobody has rerun them yet. This page separates what MiniMax published, what OpenRouter measured, and what is still open. A disclosure first: we build CellCog, an AI employee platform; our agents run on Anthropic’s Claude models (Claude Opus 5.5 on most tiers; Claude Fable 5.2 on Agent Max and Creative Max since November 11, 2026) and we do not route to MiniMax.

## On this page · 7 sectionsOpen

1. MiniMax released M3.1 Flash on October 19, 2026 and said Space Bunny Alpha, the anonymous OpenRouter model, was an earlier version of it.
2. API price: $0.15 per million input tokens and $0.60 per million output tokens, with cache reads at $0.015. OpenRouter lists it free for 72 hours first.
3. In MiniMax’s own table it scores 62.2 on Automation-Bench, ahead of DeepSeek V4.1 Flash 1017 at 60.5, and 73.7 on DeepSWE, behind that model’s 77.0.
4. It takes a 1,000,000-token context with text, image and video input, and OpenRouter allows up to 524,288 output tokens.
5. The weights landed on Hugging Face on October 23: about 428B parameters, 23B active, under a MiniMax Community License that allows commercial use with conditions.
6. OpenRouter says MiniMax upgraded the model during stealth: first token 1.95 to 1.22 seconds, 70 to 100 tokens a second, errors 1.4% to 0.7%.

## Â§ 01What MiniMax released

MiniMaxâs post says Space Bunny âwas an earlier version of M3.1 Flashâ and claims the new model runs âat 1/3 the cost of M3 and 3x the speed.â Its developer docs list MiniMax-M3.1-Flash beside MiniMax-M3, which stays available, and describe it as âAn ultra-efficient open-weight model for everyday work.â Thinking depth is set with an effort parameter.

| Item | Value | 
|---|---|
| Maker | MiniMax | 
| Teaser | October 19, 09:21 UTC | 
| Release post | October 19, 11:56 UTC | 
| Context | 1,000,000 tokens | 
| Max output on OpenRouter | 524,288 tokens | 
| Input | Text, image and video; text out | 
| API price per 1M tokens | $0.15 input, $0.60 output, $0.015 cache reads | 
| OpenRouter | Free for 72 hours, then the same rates | 
| Weights | On Hugging Face since October 23; about 428B total, 23B active | 
| License | MiniMax Community License (commercial use with conditions) | 
| Where | MiniMax API, M Plan, MiniMax Code desktop, OpenRouter | 
| Earlier name | Space Bunny Alpha, OpenRouter stealth from September 23 | 

## Â§ 02The benchmarks, as MiniMax reported them

MiniMax published one comparison table and three score-versus-cost charts with the release. The rows below are copied from that table. Every number is vendor-reported, and the table compares against GPT-5.6 Sol and Claude Opus 5, not the newer GPT-6.1 Sol and Opus 5.5.

| Benchmark | M3.1 Flash | DeepSeek V4.1 Flash 1017 | Kimi K3 | GLM-5.3 | GPT-5.6 Sol | Opus 5 | 
|---|---|---|---|---|---|---|
| Automation-Bench v1.0.6 | 62.2 | 60.5 | 46.7 | 48.8 | 45.8 | 50.3 | 
| DeepSWE v1.1 | 73.7 | 77.0 | 67.5 | 66.9 | 73.0 | 74.0 | 
| Terminal Bench 4.0 | 38.1 | 42.3 | 12.6 | 37.9 | 39.9 | 51.8 | 
| OSWorldV2 | 61.7 | not reported | 58.3 | not reported | 65.7 | 70.6 | 
| Agentsâ Last Exam | 29.6 | not reported | 28.3 | not reported | 30.6 | 32.2 | 
| BabyVision with tools | 91.0 | 92.5 | 85.7 | not reported | 88.9 | 94.1 | 
| HLE without tools | 43.4 | 42.6 | 43.5 | 42.3 | 49.5 | 55.6 | 

Three things to read before quoting these. First, M3.1 Flash leads only the agent-automation rows; on coding and terminal work DeepSeekâs October 17 checkpoint scores higher, and Opus 5 tops most rows. Second, the Automation-Bench figure appears as 62.2 in the table and 62.8% at maximum effort in MiniMaxâs cost chart, which runs the benchmark through Claude Code; OpenRouterâs launch thread cites 72.5% on a variant it calls AutomationBench-AA. Third, Opus 5.5 does appear in MiniMaxâs Agentsâ Last Exam chart, above M3.1 Flash.

## Â§ 03Cheaper per token, and cheaper per task by MiniMaxâs count

The per-token list price is half of M3âs: OpenRouter lists M3 at $0.30 in and $1.20 out per million tokens. MiniMaxâs one-third claim is about cost per task, which also depends on how many tokens the model spends. Its Automation-Bench chart puts every M3.1 Flash effort level, from low at 52.8% to max at 62.8%, left of the $0.05-per-task line, and its Agentsâ Last Exam chart prices a full run at $111.

Those cost figures come from MiniMaxâs charts, not from a third party, and the free OpenRouter window means early users will not see a bill for three days.

## Â§ 04From Space Bunny to M3.1 Flash

Space Bunny Alpha appeared on OpenRouter on September 23 with no named maker. Tokenizer tests posted that day pointed to the MiniMax family, which our Space Bunny record tracked through the reveal. OpenRouter says it was the platformâs top model for 10 straight days and processed 56.7 trillion tokens there before it âhopped off OpenRouter on Oct 5â. MiniMax counts more than 200 trillion tokens across leaderboards.

The released model is not exactly the stealth one. OpenRouter says feedback from the anonymous run let MiniMax fix bugs and ship upgrades mid-stealth:

- Time to first token fell from 1.95 to 1.22 seconds.
- Throughput rose from 70 to 100 tokens a second.
- The error rate halved, from 1.4% to 0.7%.

## Â§ 05The weights and the license (October 23)

MiniMax kept its date. The MiniMax-M3.1-Flash repository on Hugging Face was public by 01:00 UTC on October 23, 2026, with about 252 GB of 8-bit weights and recipes for SGLang and vLLM. The model card puts it at âApproximately 428B total; 23B activated per tokenâ, a mixture-of-experts model that reads text, image and video and writes text.

| Use | Terms | 
|---|---|
| Non-commercial use, copying, modifying | Free, keep the copyright notice | 
| Any commercial use | Show âBuilt with MiniMax M3.1-Flashâ on a site, UI, blog post or docs | 
| Commercial use under $20M yearly revenue | Send MiniMax a one-time notice by email | 
| Commercial use over $20M yearly revenue | Get prior written authorization from MiniMax | 
| Commercial use of the API or fine-tuned versions | Counts as commercial use under the same terms | 

It is not an OSI open-source license. The grant starts from non-commercial use, then lets commercial users in on two conditions: the attribution line, and a notice or, above $20 million a year in revenue, written permission. Fine-tuned and hosted versions count as commercial use too, and an appendix lists prohibited uses. Read the license file before you ship on it.

## Â§ 06What is still open

- **Independent numbers.** No third-party run of the released model exists yet. Browser Use and Artificial Analysis tested DeepSeekâs checkpoint within days; expect the same here.
- **Other hosts.** Together AI has teased a listing; MiniMax reposted it.
- **The stealth prompts.** OpenRouterâs stealth listing allowed the provider to retain prompts without training on them. MiniMax has not said what happens to data from the anonymous weeks.

## Â§ 07Sources

### Q1Is MiniMax M3.1 Flash free?

For now on OpenRouter: it went up as a free listing on October 19 for 72 hours. After that the listed rates are $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens, the same as MiniMax’s own API.

### Q2Is Space Bunny Alpha the same model as M3.1 Flash?

MiniMax says Space Bunny was an earlier version of M3.1 Flash. OpenRouter says user feedback during the anonymous run let MiniMax fix bugs and ship upgrades before launch, so the released model is not byte-for-byte the stealth one.

### Q3When are the M3.1 Flash weights coming?

They are out. MiniMax published them on Hugging Face on October 23, 2026, about 252 GB in 8-bit, under the MiniMax Community License, with SGLang and vLLM recipes.

### Q4How does M3.1 Flash compare with DeepSeek V4.1 Flash 1017?

On MiniMax’s own table it leads on Automation-Bench (62.2 vs 60.5) and Humanity’s Last Exam without tools (43.4 vs 42.6), and trails on DeepSWE (73.7 vs 77.0), Terminal Bench 4.0 (38.1 vs 42.3) and BabyVision (91.0 vs 92.5). No independent run exists yet.

### Q5What context window does M3.1 Flash have?

1,000,000 tokens on MiniMax’s API and on OpenRouter, which also allows up to 524,288 output tokens. It accepts text, image and video and returns text.
