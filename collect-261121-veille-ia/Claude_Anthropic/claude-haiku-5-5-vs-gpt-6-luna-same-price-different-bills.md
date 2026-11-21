---
id: collect-261121-veille-ia/claude-anthropic/claude-haiku-5-5-vs-gpt-6-luna-same-price-different-bills
title: "Claude Haiku 5.5 vs GPT-6 Luna: Same Price, Different Bills"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["haiku", "luna", "tokens", "gpt-6", "input", "anthropic", "about", "claude"]
source: https://codingfleet.com/blog/claude-haiku-5-5-vs-gpt-6-luna/
source_anchor: ""
source_lines: [1, 165]
section: "Claude_Anthropic"
---
Anthropic's **Claude Haiku 5.5** launched 15 days after OpenAI's **GPT-6 Luna**, and both list at exactly **$0.10 input / $0.50 output per million tokens** for short prompts. The rate cards match to the cent. The bills do not, and the reasons are in the tier cliffs, the token counts per task, and where each model is strongest.

### TL;DR

- **Haiku 5.5 is the more capable model on independent tests.** Artificial Analysis (AA) scores it 43 vs 38 for Luna at max effort. Haiku wins 6 of the 11 AA rows it shares with Luna, Luna wins 3, and 2 are ties.
- **Luna is about 3× cheaper per task at max effort** (AA, provisional: $0.07 vs $0.21). Haiku spends roughly 162k output tokens per AA task against about 50k for Luna.
- **Price cliffs differ.** Haiku's whole request jumps 5× once the prompt passes 100K tokens. Luna keeps its base rate up to 272K.
- **Luna leads business workflows** (AutomationBench-AA 53% vs 35%). AA flags Haiku's score as likely understated by a safety over-refusal issue that Anthropic is fixing.
- **Haiku leads computer use and terminal work** (OSWorld 2.1 offline 72.4% vs 48.9%, Anthropic-reported; Terminal-Bench 4.0 33% vs 13%, AA-measured).

both at max effort

Luna $0.07 vs Haiku $0.21

Haiku 5× vs Luna 2×

vendor-reported

## Same price card, different cliffs

Below 100K input tokens the two models are identical on every line except cache writes. Above that, Haiku's entire request is repriced. Luna keeps its base rate until 272K, then steps up by a smaller margin.

| Per 1M tokens | Haiku 5.5 ≤100K prompt | Haiku 5.5 >100K prompt | GPT-6 Luna ≤272K input | GPT-6 Luna >272K input | 
|---|---|---|---|---|
| Input | $0.10 | $0.50 | $0.10 | $0.20 | 
| Output | $0.50 | $2.50 | $0.50 | $0.75 | 
| Cache read | $0.01 | $0.05 | $0.01 | $0.02 | 
| Cache write (5 min) | $0.125 | $0.625 | $0.125 | $0.25 | 

**Cost of one request at list prices, no caching.**Identical up to 100K tokens. At 150K input, Haiku costs 5× more per request. Calculated from the list rates above. Excludes thinking tokens, retries, tool fees and Batch/Flex discounts.

**Where the cliff bites.** Anthropic says about 90% of requests to the previous Haiku stayed under 100K tokens. If your traffic looks like that, the step rarely matters. A contract-review job at 150K tokens per document, run 10,000 times a month, costs about **$800 on Haiku 5.5 vs $160 on Luna**. A 99K prompt on Haiku costs about $0.011; a 101K prompt costs about $0.056.

## Specifications

| Specification | Claude Haiku 5.5 | GPT-6 Luna | 
|---|---|---|
| Released | 7 October 2026 | 22 September 2026 | 
| API model ID | `claude-haiku-5-5` | `gpt-6-luna` | 
| Context window | 1M tokens | 1.05M tokens | 
| Max output | 128K tokens | 128K tokens | 
| Knowledge cutoff | June 2026 | 18 May 2026 | 
| Effort levels | low, medium, high, xhigh, max (default medium) | none, low, medium, high, xhigh, max (default medium) | 
| Input / output | Text and images / text | Text and images / text | 
| Tokenizer | Updated; uses more tokens than Haiku 4.5 for the same text | OpenAI tokenizer | 

## Vendor-reported benchmarks

Anthropic's launch table is the only place both models appear side by side with vendor numbers, and Anthropic wrote it. Haiku leads every row where both models are reported. Treat these as Anthropic's evaluation setup, not an independent result.

| Benchmark | Haiku 5.5 | GPT-6 Luna | Haiku 4.5 | Sonnet 5.5 (ref) | 
|---|---|---|---|---|
| GDPval-AA v2.1 knowledge work · Elo | 1,620 | 1,437 | 735 | 1,840 | 
| AA-Briefcase v1.1 work documents · Elo | 1,578 | 1,336 | 614 | 1,824 | 
| OSWorld 2.1 computer use · offline subset | 72.4% | 48.9% | 15.7% | 83.9% | 
| Terminal-Bench 4.0 agentic CLI work | 39.2% | 16.4% | 0.0% | 70.6% | 
| FrontierCode 1.1 (Main) mergeable code | 46.4% | 42.4% | — | 52.1% (xhigh) | 
| Chartography visual reasoning · no tools | 46.4% | 29.1% | 6.4% | 61.6% | 
| Humanity's Last Exam no tools | 45.9% | — | 10.2% | 56.9% | 

**Vendor-reported radar.**Elo scores normalised as (Elo − 500) / 2000, the convention AA uses. Luna's values are Anthropic's, not OpenAI's.

**Independent radar (Artificial Analysis).**Hallucination is shown inverted as non-hallucination rate, so bigger is better on every axis.

Two rows exist on only one side. Anthropic reports **SWE-bench Pro at 64.8%** and **HLE with tools at 57.4%** for Haiku, and OpenAI does not publish either for Luna. OpenAI reports **DeepSWE v1.1 at 66.6%** for Luna at max effort, and Anthropic does not report it for Haiku.

## Independent results: Artificial Analysis

AA runs both models through the same Intelligence Index v4.3.2 harness, so this is the fairer table. Both are at max effort.

| AA evaluation | Haiku 5.5 (max) | GPT-6 Luna (max) | Reading | 
|---|---|---|---|
| Intelligence Index 10 evals, v4.3.2 | 43 | 38 | Haiku +5 points | 
| GDPval-AA v2.1 Elo | 1,620 | 1,432 | Haiku ahead | 
| AA-Briefcase v1.1 Elo | 1,578 | 1,336 | Haiku ahead; within CI of GPT-6 Astra (max) | 
| Humanity's Last Exam | 44% | 39% | Haiku ahead | 
| Terminal-Bench 4.0 | 33% | 13% | Haiku ahead, up from 0% for Haiku 4.5 | 
| AutomationBench-AA business workflows | 35% | 53% | Luna ahead; AA says Haiku is understated | 
| GDP.pdf document reasoning | 21% | 23% | Luna, narrowly | 
| SciCode | 55% | 55% | Tie | 
| AA-LCR v1.1 long context | 83% | 83% | Tie | 
| AA-Omniscience accuracy | 36% | 44% | Luna answers more questions correctly | 
| AA-Omniscience hallucination rate lower is better | 40% | 77% | Haiku abstains and invents less | 

**Read the AutomationBench row with care.** During pre-release testing a safety refusal issue made Haiku over-refuse. AA says it will re-run the evaluation after Anthropic's fix and expects the score to rise. Until then, Luna's lead on business workflows is measured, and Haiku's gap may shrink.

## Effort is the real dial

Haiku 5.5 is the first Haiku with an effort setting, and that changes the comparison. Set to `high`, it ties Luna's max-effort score on AA. The cost gap then nearly closes, because Haiku needs about 55k output tokens per task to Luna's 50k.

**AA Intelligence Index by Haiku effort level.**Dashed line: GPT-6 Luna at max (38). Haiku's medium default scores 34.

**Output tokens per AA Intelligence Index task (thousands).**Haiku at max uses about 3× the tokens Luna does for the whole index.

| Setting | AA index | Output tokens / task | Cost / task (AA, provisional) | Verdict | 
|---|---|---|---|---|
| Haiku 5.5 medium (default) | 34 | — | $0.05 | Cheapest Haiku; below Luna's 38 | 
| Haiku 5.5 high | 38 | ~55k | $0.08 | Matches Luna max on score, about the same cost | 
| Haiku 5.5 max | 43 | ~162k | $0.21 | Only Haiku setting above 41; costly | 
| GPT-6 Luna max | 38 | ~50k | $0.07 | Cheapest route to a score of 38 | 

**About the cost figures.** AA says its Haiku cost numbers do not yet include the step-up above 100K tokens, so they are provisional and may rise on long-prompt tasks. The medium and high figures come from secondary write-ups of AA's data. The token counts and index scores are AA's.

## Speed and latency

Haiku streams faster once it starts. Luna starts sooner at max effort, because Haiku's thinking phase is much longer there.

| Metric (AA, max effort) | Haiku 5.5 | GPT-6 Luna | Reading | 
|---|---|---|---|
| Output speed | 243 tok/s | 127 tok/s | Haiku streams about 2× faster | 
| Time to first answer token | 323 s | 109 s | Luna answers sooner at max effort | 
| Time to first token, medium | ~14 s | n/a | Haiku's medium setting responds quickly | 

AA snapshots move as it re-runs tests. The max-effort numbers are long benchmark tasks, not chat latency, so measure your own requests. Anthropic's customer quotes (Asana reports over 30% lower task-completion latency against its previous model; Box reports about half the latency of Haiku 4.5) are vendor testimonials, not tests.

## Routing guide

| Your workload | Pick | Why | 
|---|---|---|
| Cheapest per task at an index score of 38 | GPT-6 Luna (max) | $0.07 vs about $0.08 for Haiku at high. Close to a tie. | 
| Anything needing an index score above 41 | Haiku 5.5 (max) | Luna's ceiling on AA is 38. Haiku reaches 43. | 
| Documents between 100K and 272K tokens | GPT-6 Luna | Five times cheaper per token in that band. | 
| Multi-step business automation across tools | GPT-6 Luna | 53% vs 35% on AutomationBench-AA, pending AA's re-run of Haiku. | 
| Browser and desktop agents | Haiku 5.5 | 72.4% vs 48.9% on OSWorld 2.1 offline, vendor-reported. | 
| Live chat, voice and support replies | Haiku 5.5 at medium or high | Responds in seconds at those settings; Luna's max-effort first-token wait is over 100 s on AA. | 
| Grounded answers from supplied sources | Test both | Haiku hallucinates less (40% vs 77%); Luna answers more correctly (44% vs 36%). Pick by your error tolerance. | 
| Coding agents | Haiku 5.5 (marginal) | Ahead on FrontierCode and Terminal-Bench in both vendor and AA data. Neither replaces Sonnet 5.5 or Opus 5.5 for hard tasks. | 

### The short version

**Haiku 5.5 is the more capable model, and Luna is the cheaper one per unit of work.** If the job needs the top of the small-model range (index above 40), computer use, or terminal work, Haiku is the better choice even at max effort. If the job is business automation, or long documents between 100K and 272K tokens, Luna gives you more for the money.

If your traffic is short prompts and a score of about 38 is enough, the two are nearly identical in cost. Then the deciding factors are latency and failure behaviour. Run 200 real requests through both before you commit.

## FAQ

## Is Claude Haiku 5.5 better than GPT-6 Luna?

On capability, yes. Artificial Analysis scores Haiku 43 and Luna 38 at max effort, and Haiku leads Anthropic's vendor table on every shared row. Luna wins on business workflows and on cost per task at max effort, and it has the larger window and the longer 272K price band.

## Which is cheaper?

For prompts up to 100K tokens, the list prices are identical. Per AA task at max effort, Luna costs about $0.07 and Haiku about $0.21, a 3× gap driven by Haiku's higher token use. Above 100K tokens Luna is far cheaper per token until 272K.

## Why does Haiku use so many tokens?

Most of the extra output is reasoning. At max effort Haiku uses roughly 162k output tokens per AA Intelligence Index task, against about 50k for Luna. Moving from xhigh to max buys Haiku 2 index points for about 1.8× the tokens, so max is rarely worth it for high-volume work.

## Can I use Haiku 5.5 for coding?

Anthropic positions it for subagent and narrow tasks, and says Sonnet 5.5 and Opus 5.5 are the better choice for complex agentic coding. Haiku does lead Luna on FrontierCode and Terminal-Bench in both vendor and AA data, but the margins are small and the absolute scores are well below the larger models.

## Sources

- Anthropic — Introducing Claude Haiku 5.5: vendor benchmark table, pricing, the 100K tier, effort settings.
- Artificial Analysis — Anthropic has released Claude Haiku 5.5: Intelligence Index 43, token use, AA-Omniscience, AutomationBench caveat, provisional cost note.
- Artificial Analysis — Claude Haiku 5.5 (Max) vs GPT-6 Luna (Max): speed and time-to-first-token figures.
- Artificial Analysis — GPT-6 Sol and Luna push the cost efficiency frontier: Luna's AA results, $0.07 per task, 50K tokens, hallucination and accuracy.
- OpenAI — Introducing GPT-6 Sol and Luna: Luna pricing, DeepSWE v1.1 66.6%, AutomationBench framing.
- OpenRouter — Claude Haiku 5.5: AA data for Haiku at low, medium, high and max effort.
- Emergent — Claude Haiku 5.5 vs GPT-6 Luna: effort-level cost figures and the Luna 272K tier cross-check.
- LLM Stats — SWE-Bench Pro leaderboard: Haiku 5.5 at 64.8%.
- The New Stack — Anthropic launches Haiku 5.5 at a much lower price: launch coverage and pricing cross-check.

Vendor scores are labelled as vendor-reported. AA scores are independent runs in AA's harness at max effort unless stated. Where secondary sources disagreed, this article uses the primary or AA figure and omits the rest. OpenAI's OSWorld 2.0 figure for Luna is reported inconsistently across secondary sources (52.7% and 58.1%), so it is left out. Prices are USD per million tokens at list rates, October 2026.
