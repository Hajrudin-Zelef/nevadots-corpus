---
id: collect-261121-veille-ia/kimi/deepseek-vs-claude-gemini-gpt-kimi-nov-2026-dseek-app
title: "DeepSeek vs Claude, Gemini, GPT & Kimi (Nov 2026) | dseek.app"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["deepseek", "claude", "artificial", "analysis", "gemini", "index", "kimi", "tokens"]
source: https://dseek.app/guides/deepseek-vs-claude-gemini-gpt
source_anchor: ""
source_lines: [1, 224]
section: "Kimi"
---
# DeepSeek vs Claude, Gemini, GPT & Kimi: benchmarks, prices, usage (Nov 2026)

DeepSeek V4.1-Flash is the cheapest and fastest model in this set: $0.60 per Artificial Analysis index task and 171 tokens per second, against $26.54 for Claude Fable 5.2 at Max. It is also #1 by tokens on OpenRouter’s rankings for the week to 14 November 2026.

It is not frontier-level. Flash scores 33 on the Artificial Analysis Intelligence Index v5.0.1, against 65 for Claude Fable 5.2, 60 for Gemini 4 Carbon, 57 for GPT-6 Astra and 46 for Kimi K3.1 Pro. On the current index, Flash also outscores DeepSeek V4-Pro (23).

Every figure below comes from public trackers (Artificial Analysis, LMArena, OpenRouter) or the makers’ own pricing pages, checked on 15 November 2026. dseek.app did not run these tests.

## Official sources

## How these figures were collected

- **Official prices** come from each maker’s API pricing page (USD per 1M tokens). For DeepSeek that is the API docs pricing page, not the marketing page, which still showed older, lower figures.
- **Intelligence, speed, latency and cost per task** come from Artificial Analysis, Intelligence Index v5.0.1. Artificial Analysis runs the evaluations itself; speed and latency are recent averages and move day to day.
- **Human-preference ranks** come from the LMArena Text Arena board dated 11 November 2026.
- **Usage** comes from OpenRouter’s rankings, Hugging Face download counts, and DeepSeek’s own published Harness figures (labelled as DeepSeek’s).
- Each model is shown at its **highest published Artificial Analysis setting** (Max, or High where Max is not published). Effort changes results a lot.
- The **3:1 blended price** (three input tokens to one output token, no cache) is our own calculation from official list prices. Real bills differ with cache hits, thinking tokens, long-context surcharges and time-of-day rates.

## Models compared

DeepSeek’s two current official API models, `deepseek-flash` (V4.1-Flash-1017) and `deepseek-v4-pro` (V4-Pro-0813), against the top two models on the Artificial Analysis index (Claude Fable 5.2, Gemini 4 Carbon), OpenAI’s flagship and near-flagship (GPT-6 Astra, GPT-6.1 Sol), and the highest-ranked open-weight model (Kimi K3.1 Pro). Left out for now: Claude Opus and Sonnet 5.5, other Gemini 4 variants, Grok 4.7 (index 35) and GLM-5.3 (index 32).

## Headline comparison

Checked 15 Nov 2026. DeepSeek is in the first two columns. Scroll sideways on small screens.

| Headline comparison of DeepSeek and rival models |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|
| Metric | DeepSeek V4.1-Flash (Max) | DeepSeek V4-Pro (Max) | Claude Fable 5.2 (Max) | Gemini 4 Carbon (High) | GPT-6 Astra (Max) | GPT-6.1 Sol (Max) | Kimi K3.1 Pro | Source | 
|---|---|---|---|---|---|---|---|---|
| Maker / API name | DeepSeek / deepseek-flash | DeepSeek / deepseek-v4-pro | Anthropic / claude-fable-5-2 | Google / gemini-4-carbon | OpenAI / gpt-6-astra | OpenAI / gpt-6.1-sol | Moonshot / kimi-k3.1-pro | Artificial Analysis + maker docs | 
| AA Intelligence Index v5.0.1 | 33 | 23 | 65 | 60 | 57 | 55 | 46 | Artificial Analysis leaderboard | 
| LMArena Text Arena rank (Elo), 11 Nov 2026 board | #39, 1467±8 — listed as the 20260910 build, not 1017 | #51, 1450±5 (generic “DeepSeek-V4-Pro” label) | Not published (5.2 not on the board yet; Fable 5.1 Max is #4) | #1, 1564±13 (preliminary) | #79, 1435±7 | #63, 1442±8 | #43, 1462±13 | LMArena Text Arena | 
| Official input / output, USD per 1M tokens | Peak $0.30 / $1.20; off-peak $0.15 / $0.60 | Peak $1.32 / $3.96; off-peak $0.66 / $1.98 | $8 / $40 | $2 / $10 to 30 Nov 2026; $4 / $20 from 1 Dec 2026 | $10 / $50 (≤272K input); long context $20 / $75 | $2 / $10 (short context); long context $4 / $15 | $2 / $10 (cross-checked, see pricing note) | Maker pricing pages (see price table) | 
| Official cache-hit input, USD per 1M | Peak $0.006; off-peak $0.003 | Peak $0.044; off-peak $0.022 | $0.20 | $0.10 to 30 Nov; $0.20 from 1 Dec (implicit cache only) | $1.00 (short context) | $0.10 (short context) | $0.20 (cross-checked) | Maker pricing pages (see price table) | 
| Calculated 3:1 blend (3 input : 1 output, no cache) | Peak $0.525; off-peak $0.2625 | Peak $1.98; off-peak $0.99 | $16.00 | $4.00 intro; $8.00 from 1 Dec | $20.00 (short context) | $4.00 (short context) | $4.00 | Calculated by dseek.app from official list prices | 
| AA cost per Intelligence Index task | $0.60 | $1.77 | $26.54 | $5.48 | $6.24 | $1.27 | $3.68 | Artificial Analysis | 
| AA output speed | 171 tok/s | 91 tok/s | 102 tok/s | 61 tok/s | 40 tok/s | 49 tok/s | 30 tok/s | Artificial Analysis | 
| AA time to first answer token (includes thinking) | 44.55 s | 51.48 s | 262.07 s | 72.06 s | 230.28 s | 190.80 s | 178.90 s | Artificial Analysis | 
| AA time per task | 13.5 min | 18.1 min | 44.5 min | 33.8 min | 21.8 min | 22.9 min | 40.1 min | Artificial Analysis | 
| Context window (official) | 1M | 1M | 1M | 1M | 1,050,000 | 1,050,000 | 1,048,576 | Maker docs | 
| Max output (official) | 384K | 384K | 128K | Up to 1M via continuation | 128K | 128K | Not published | Maker docs | 
| Image input | Yes | No (text only) | Yes | Yes (plus speech and video) | Yes | Yes | Yes | Maker docs | 
| Weights / licence | Open weights, MIT (552B total / 16B active) | Open weights, MIT (1.6T total / 49B active) | Proprietary | Proprietary | Proprietary | Proprietary | Open weights, Kimi licence (terms not independently verified) | Artificial Analysis + Hugging Face | 

## Official API prices

USD per 1M tokens, checked 15 Nov 2026. DeepSeek off-peak rates are half of peak. Peak hours are 01:00–04:00 and 06:00–10:00 UTC, Monday to Friday, excluding Chinese public holidays (see the off-peak guide).

| DeepSeek model | Cache hit (off-peak / peak) | Input, cache miss (off-peak / peak) | Output (off-peak / peak) | 
|---|---|---|---|
| deepseek-flash (V4.1-Flash-1017) | $0.003 / $0.006 | $0.15 / $0.30 | $0.60 / $1.20 | 
| deepseek-v4-pro (V4-Pro-0813) | $0.022 / $0.044 | $0.66 / $1.32 | $1.98 / $3.96 | 

| Rival model | Input | Output | Cache hit | Notes | Source | 
|---|---|---|---|---|---|
| Claude Fable 5.2 | $8 | $40 | $0.20 | 5-minute cache write $10; 1-hour write $16. Batch 50% off. | Anthropic pricing | 
| Gemini 4 Carbon | $2 to 30 Nov; $4 from 1 Dec 2026 | $10 to 30 Nov; $20 from 1 Dec 2026 | $0.10 to 30 Nov; $0.20 from 1 Dec 2026 | Paid tier. Implicit caching only. Output price includes thinking tokens. | Gemini Developer API pricing | 
| GPT-6 Astra | $10 short; $20 long (>272K input) | $50 short; $75 long | $1.00 short; $2.00 long | Cache writes $12.50 (30 min) / $25 (12h). Batch and Flex 50% of Standard; Fast 2×. | OpenAI pricing | 
| GPT-6.1 Sol | $2 short; $4 long | $10 short; $15 long | $0.10 short; $0.20 long | Cache writes $2.50 (30 min) / $5 (12h). Same Batch / Flex / Fast pattern as Astra. | OpenAI pricing | 
| Kimi K3.1 Pro | $2.00 | $10.00 | $0.20 | The official price table did not render as text when checked, so these rates are cross-checked against Artificial Analysis, OpenRouter’s Moonshot AI endpoint and Kimi’s pricing article. Highspeed variant $4 / $20. | Kimi pricing page | 

## Artificial Analysis index and component scores

Intelligence Index v5.0.1, checked 15 Nov 2026. The index combines nine evaluations (methodology).

| Artificial Analysis Intelligence Index component scores |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|
| Metric | DeepSeek V4.1-Flash (Max) | DeepSeek V4-Pro (Max) | Claude Fable 5.2 (Max) | Gemini 4 Carbon (High) | GPT-6 Astra (Max) | GPT-6.1 Sol (Max) | Kimi K3.1 Pro | Source | 
|---|---|---|---|---|---|---|---|---|
| Intelligence Index v5.0.1 | 33 | 23 | 65 | 60 | 57 | 55 | 46 | Artificial Analysis | 
| AA-Briefcase v1.1 (Elo) | 1570 | 1253 | 1785 | 1611 | 1569 | 1555 | 1674 | Artificial Analysis | 
| AutomationBench-AA | 66.6% | 56.7% | 76.4% | 77.8% | 68.5% | 64.9% | 73.3% | Artificial Analysis | 
| Terminal-Bench 4.0 | 33.8% | 14.1% | 59.6% | 65.2% | 59.1% | 56.1% | 67.2% | Artificial Analysis | 
| FrontierCode-AA 2.0 | 28.5% | 17.1% | 48.8% | 40.6% | 42.4% | 40.6% | 25.5% | Artificial Analysis | 
| AA-Omniscience v1.1 (Net Score, −100 to 100) | 1 | −6 | 67 | 54 | 42 | 42 | 30 | Artificial Analysis | 
| GDP.pdf | 17.2% | 11.4% | 24.2% | 28.4% | 31.0% | 31.0% | 31.4% | Artificial Analysis | 
| HLE-Diamond | 25.3% | 19.5% | 80.2% | 69.1% | 63.8% | 58.8% | 37.3% | Artificial Analysis | 
| CritPt v2 (hard) | 25.0% | 23.5% | 76.5% | 69.0% | 70.5% | 69.5% | 49.5% | Artificial Analysis | 
| Terminal-Bench-Science 0.1 | 12.9% | 5.7% | 73.3% | 64.8% | 63.3% | 58.1% | 34.3% | Artificial Analysis | 

### How to read the DeepSeek scores

On this index, **Flash outscores Pro** (33 vs 23), and it is cheaper and faster per task too. The gap to the leaders is widest on agentic coding and science (Terminal-Bench, FrontierCode, Terminal-Bench-Science). Flash is more competitive on AA-Briefcase (1570, close to Astra’s 1569 and Sol’s 1555) and AutomationBench (66.6%). Kimi K3.1 Pro leads the open-weight group and, unusually, the whole set on Terminal-Bench 4.0 (67.2%); Flash slightly leads Kimi on FrontierCode (28.5% vs 25.5%).

Often overlooked: the Omniscience test

AA-Omniscience is easy to misread, because a score near zero looks like a fail. It is not an accuracy percentage. The Net Score runs from −100 to 100: correct answers add points, confident wrong answers (hallucinations) take points away, and saying “I don’t know” is neutral, so declining to guess beats guessing wrongly. A score of 0 means about as many right answers as wrong ones, not that the model got everything wrong.

On that scale, Flash scores **1** and Pro **−6**: right and confident-wrong answers roughly cancel. Claude Fable 5.2 at **67** and Gemini 4 Carbon at **54** get far more right than wrong, or hold back when unsure.

**Practical takeaway:** for hard or high-stakes factual tasks, plan for a person reviewing the answers or escalate to a stronger model. Routine work is still a good fit for Flash.

How to work around it

The Omniscience net score penalises wrong answers, while “not attempted” is neutral, so a low score comes from confident guessing rather than from leaving a question blank. See the AA-Omniscience v1.1 evaluation page.

- Ground answers in sources you supply, through retrieval or DeepSeek’s Search & Fetch API, so the model reads the facts instead of recalling them from memory.
- Allow “I don’t know” in the system prompt. Declining to guess is better than a confident wrong answer on this test.
- Require a citation for each factual claim, and treat unsourced claims as incomplete.
- Route knowledge-heavy or high-stakes factual questions to a higher-accuracy model, and keep Flash for high-volume work such as coding, summaries and extraction.
- Add a verification pass that checks answers against the sources before they go out.
- Keep a person in the loop for legal, medical, financial or public-facing facts.

Flash costs **$0.60** per Artificial Analysis index task, against **$1.27** for Sol, **$5.48** for Carbon and **$26.54** for Fable. That leaves budget for retrieval and a check step while staying cheaper overall.

### Other published effort settings

- Flash 1017: High 29; non-reasoning 19. V4-Pro 0813: High 21; non-reasoning 13.
- Fable 5.2: xhigh 62; high 62; medium 56; low 54 (low costs $3.10 per task and takes 5.6 min).
- Carbon: medium 58; low 54. Astra: xhigh 55; high 55; medium 52; low 43.
- Sol: xhigh 52; high 50; medium 46; low 35 (low costs $0.16 per task). Kimi: one published row only.

**Do not mix index versions.** An earlier Artificial Analysis article scored Pro (Max) at 52 and Flash (Max) at 47 on a previous index; those numbers are not comparable with v5.0.1.

## LMArena human-preference ranks

Text Arena, exclude ties, raw scoring. Board dated 11 November 2026 (6,483,712 votes, 421 models).

| Model as listed | Rank | Elo | Votes | Notes | 
|---|---|---|---|---|
| Gemini 4 Carbon (High) | 1 | 1564±13 | 3,309 | Preliminary | 
| Claude Fable 5.2 | — | Not published | — | Too new for this board. Claude Fable 5.1 (Max) is #4 at 1531±7. | 
| DeepSeek-V4.1-Flash-20260910 (Max) | 39 | 1467±8 | 9,928 | Not the current 1017 build — treat as a related snapshot. | 
| Kimi K3.1 Pro (Max) | 43 | 1462±13 | 2,974 |  | 
| DeepSeek-V4-Pro | 51 | 1450±5 | 43,076 | Generic label; may not be the 0813 build. | 
| GPT-6.1 Sol (Max) | 63 | 1442±8 | 9,949 |  | 
| DeepSeek-V4-Pro (High, 20260813) | 66 | 1441±9 | 7,025 | High, not Max. | 
| GPT-6 Astra (Max) | 79 | 1435±7 | 13,939 |  | 

This board and the Artificial Analysis index disagree. OpenAI’s flagships rank much lower here (Astra #79, Sol #63) than on the index, while DeepSeek Flash’s snapshot (#39) sits near Kimi and above Astra. They measure different things, so neither cancels the other.

## Gaps and conflicts between sources

- Flash beats Pro on the current index, cost and speed. DeepSeek’s own OpenRouter model blurb says the same; that maker claim was not independently tested here.
- LMArena has no Fable 5.2 row yet, and its Flash row is the 20260910 snapshot, not 1017.
- Gemini 4 Carbon’s official prices rise to $4 / $20 on 1 December 2026. Artificial Analysis and OpenRouter still used $2 / $10 on 15 November.
- OpenAI context length is 1,050,000 in the official docs, 1M on Artificial Analysis and 1.1M on some OpenRouter pages.
- Gemini cache-write pricing differs by source; Artificial Analysis listed $4.00, which we do not treat as official.
- OpenRouter headline prices for open-weight models are often the cheapest host, not the official list (Kimi $0.24 / $8.40 vs $2 / $10; DeepSeek Flash $0.05 / $0.20 vs $0.30 / $1.20 peak).
- The generic OpenRouter slug `deepseek/deepseek-v4-pro` is the older 0423 snapshot; the current Pro is`deepseek/deepseek-v4-pro-0813` .
- No first-hand quality, speed or price measurements, and no self-hosting costs, are included.

## Where DeepSeek fits

### Strong fits

- **High-volume, cost-sensitive work.** Flash lists at $0.30 / $1.20 per 1M tokens at peak, against $2 / $10 for GPT-6.1 Sol and $8 / $40 for Claude Fable 5.2. Its cost per index task is $0.60, against $1.27 for Sol, $5.48 for Carbon and $26.54 for Fable.
- **Batch work that can wait for off-peak hours,** which halves the official price.
- **Routine coding help, classification and automation.** Flash’s AA-Briefcase (1570) is close to Astra and Sol, and its AutomationBench score (66.6%) is slightly above Sol’s, though behind Carbon and Fable.
- **Interactive loops where waiting matters.** 1.25 s to first token and 171 tok/s, against 3–4 minutes to first answer for Fable, Astra and Sol at Max.
- **Image input** (Flash only) and**self-hosting** : both models are open weights under MIT on Hugging Face.

### Pair it with a frontier model

The overall gap is large: Flash scores 33 against 46–65 for the rivals, and only 33.8% on Terminal-Bench 4.0 against 59.6–67.2%. A practical pattern is to send most volume to Flash and pass hard or high-stakes cases up to a frontier model, or to Kimi for an open-weight step-up. A lighter frontier setting also works: Fable at Low still scores 54 at $3.10 per task. And on the current index, do not assume V4-Pro is the smarter DeepSeek default.

### Where to be careful or add safeguards

- **Hard or high-stakes factual answers.** See the Omniscience note: plan for human review, add retrieval or citations, or escalate to a stronger model.
- **Legal, medical, safety-critical or hard-to-undo decisions.** Flash’s HLE-Diamond (25.3%) and Terminal-Bench-Science (12.9%) scores, against Fable’s 80.2% and 73.3%, do not support using DeepSeek as the only checker.
- **The hardest agentic coding, used alone.** Fine as a first pass or cheap worker; weak as the only model on long, tool-heavy jobs.
- **Secrets and sensitive personal data.** DeepSeek’s Open Platform privacy policy (Chinese, effective 11 Nov 2026) tells developers not to submit sensitive personal information, credentials or similar data unless necessary and lawful.
- **Data residency and training use.** The official API is provided by a company registered in China (Open Platform Terms). The Open Platform privacy policy says personal information from domestic operations is stored in the PRC, and that, without a separate written agreement, DeepSeek may use API inputs and outputs to improve models after encryption and de-identification. Teams with residency or sector rules should read the official policies and take their own advice; third-party hosts and self-hosting are alternatives with their own terms.

## Usage rankings and what they mean for production apps

Usage shows whether a model already runs in real applications. It is **not a quality score**: OpenRouter says its rankings measure tokens or requests on its own network only.

### OpenRouter tokens processed

| Period (to 14 Nov 2026) | DeepSeek V4.1 Flash 1017 | Next DeepSeek variant | Notes | 
|---|---|---|---|
| Most recent complete day | 8.09T (#1) | V4.1 Flash 2.01T (#3) | Day-on-day change for 1017: −10%. | 
| Trailing 7 days | 58T (#1, +4%) | V4.1 Flash 15.7T (#3, −11%) | Hy4 is #2 at 18.9T. GPT-6.1 Sol is #10 at 3.89T. GPT-6 Astra, Claude Fable 5.2, Gemini 4 Carbon and V4-Pro were not in the weekly top 10. | 
| Trailing 30 days | 193T (#1, listed as new) | V4.1 Flash 76.2T (#2) |  | 

Source: OpenRouter rankings, checked 15 Nov 2026; private apps are excluded. On request share for the week beginning 2 November 2026, DeepSeek had 31.8% of text requests, ahead of OpenAI 19.3%, Google 13.6%, Z.ai 7.4% and Anthropic 5.8%. A third-party tracker, Frenzy Capital, calculates DeepSeek at 38.2% of OpenRouter tokens; that percentage is its calculation, not an official OpenRouter figure.

The top public apps on OpenRouter are mostly coding and general agents (Hermes Agent, Claude Code, Kilo Code, pi, Codex), the kind of workload where Flash is already cheap and fairly competitive.

### DeepSeek’s own Harness figures

These are **DeepSeek’s own figures** as of 4 November 2026, for the official API only, not independently audited, and taken from reports of the announcement: 1.54 million daily active users, about 92 trillion tokens a day, and about 35,000 third-party plugins (TechNode, IT Home, SCMP).

### Hugging Face downloads

| Model repo | Downloads | Likes | 
|---|---|---|
| deepseek-ai/DeepSeek-V4.1-Flash-1017 | 460,551 | 5,727 | 
| deepseek-ai/DeepSeek-V4.1-Flash | 1,694,181 | 4,977 | 
| deepseek-ai/DeepSeek-V4-Flash-0731 | 2,235,061 | 4,107 | 
| deepseek-ai/DeepSeek-V4-Pro-0813 | 52,432 | 916 | 
| deepseek-ai/DeepSeek-V4-Pro | 318,350 | 5,712 | 

### What this means for production

- **Adoption is high, especially for Flash.** Many apps already depend on it, and there is a wide host and plugin ecosystem. That is a supply signal, not evidence that Flash matches Fable or Carbon (index 33 vs 65 and 60).
- **OpenRouter is only one window.** DeepSeek’s reported 92T tokens a day on its own API dwarfs OpenRouter’s 8.09T for Flash on its latest day, so most production use appears to go straight to DeepSeek.
- **Fallback hosts exist, but differ.** Artificial Analysis listed 21 API providers for Flash 1017 and 8 for Pro 0813. Third-party discounts can change, and the MIT weights add a self-host fallback.
- **Rate limits are concurrency caps.**`deepseek-flash` allows 2,500 concurrent requests per account and`deepseek-v4-pro` 500; over the limit returns HTTP 429 (Rate Limit & Isolation; see also API errors).
- **Uptime is published per host.** On OpenRouter’s Pro 0813 page, the first-party DeepSeek row showed 100.00% uptime and Cloudflare 87.92%; most other hosts were in the high 99s. We found no single official DeepSeek SLA.
- **Usage is not quality.** The same week Flash led OpenRouter by tokens, it scored 33 on the index. That supports a “cheap default plus stronger fallback” design more than replacing a frontier model outright.

## Full source list

All checked on 15 Nov 2026 unless the source itself is dated earlier.

## FAQ

Not on overall intelligence. On Artificial Analysis Index v5.0.1, V4.1-Flash scores 33 against 55–65 for the GPT, Gemini and Claude models compared here.

Its strengths are price and speed: $0.60 per index task and 171 tok/s, the lowest cost and highest speed in this set. A common pattern is Flash as the cheap default, with hard cases passed to a frontier model.

On the current public figures, Flash. It scores 33 against Pro’s 23 on Index v5.0.1, costs $0.60 per task against $1.77, and is faster.

No. AA-Omniscience Net Score runs from −100 to 100 and penalises confident wrong answers, so 0 means as many right answers as wrong ones.

Ground answers in sources you supply, allow the model to say it does not know, require citations, and add a verification pass or human review for high-stakes facts.

DeepSeek V4.1-Flash 1017 was #1 by tokens on OpenRouter for the 7 and 30 days to 14 November 2026 (58T and 193T tokens).

No. Every figure is copied from a public tracker or a maker’s pricing or docs page, with the source linked.
