---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-vs-gpt-6-1-sol-astra-and-fable-5-1
title: "Claude Sonnet 5.5 vs GPT-6.1 Sol, Astra and Fable 5.1"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["gpt-6", "sonnet", "fable", "model", "astra", "more", "claude", "against"]
source: https://www.intelligentliving.co/claude-sonnet-5-5-vs-gpt-6-sol-astra/
source_anchor: ""
source_lines: [1, 345]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5 launched on September 28, 2026, at $2 per million input tokens and $10 per million output tokens. Hours later, OpenAI replaced GPT-6 Sol with GPT-6.1 Sol, which scores 52 on the Artificial Analysis Intelligence Index against Sonnet 5.5’s 56. The gap is four points, where it was eight the day before.

The per-benchmark detail is messier and more useful, and it now comes from two independent suites. Sonnet 5.5 beats GPT-6.1 Sol on seven of eleven Artificial Analysis evaluations and fifteen of nineteen Vals AI benchmarks. It beats its own sibling, Fable 5.1, on 8 and loses on 10, despite winning the composite. The wins cluster in code migration, terminal work, and web app building. The losses cluster in finance, legal, tax, medical coding, and scientific research.

There is also a finding that has nothing to do with speed or price. Sonnet 5.5 holds the highest non-hallucination rate in the field at 52.99%, and the lowest knowledge accuracy at 53.95%. GPT-6.1 Sol inverts both: 62% accuracy, second only to Fable 5.1, alongside a 54.27% hallucination rate. The model that answers more is wrong more, and the model that declines most is wrong least.

So this is not a general improvement. It is a specialization, and the right way to buy is to route work rather than pick a winner. The headline result is simultaneously real and misleading: Sonnet 5.5 wins most of the benchmarks, and costs roughly ten times more to finish the same job, because the two models consume wildly different numbers of tokens to get there. That tension runs through everything below. This article sets Sonnet 5.5 against the four models it competes with, using every publicly available figure across Vals AI and Artificial Analysis, plus Anthropic’s and OpenAI’s own claims. Where a figure could not be confirmed against a primary source, the cell is left blank rather than guessed.

One naming note. **GPT-6.1 Sol** is the current OpenAI mid-tier at $2 and $10, and it supersedes GPT-6 Sol, which launched on September 22 and is no longer the model anyone should be pricing against. **GPT-6 Astra** is OpenAI’s flagship at $10 and $50. They are different products separated by one price tier. We covered the Sol family’s launch in our GPT-6 Sol and Luna benchmark breakdown and Astra against the Claude flagships in our GPT-6 Astra comparison.

There is no single winner, and the reason is more useful than a verdict. Six findings, in the order they should change your decisions.

- **A mid-tier model caught the flagships, and stayed caught.** Sonnet 5.5 still outscores Claude Fable 5.1 and GPT-6 Astra on the Artificial Analysis index while costing a fifth as much per token. GPT-6.1 Sol closed most of the gap to Sonnet but not past it.
- **The composite index is misleading on its own.** Sonnet 5.5 edges past Fable 5.1 on the Vals composite by a fraction of a point, then loses the head-to-head 8 to 10. The composite is being carried by coding benchmarks, not by the professional work Fable is bought for.
- **Astra owns the scientific tail, but no longer alone.** It is the only model in the field to finish IOI with a perfect score, and it leads SRE Bench and Terminal-Bench Science. GPT-6.1 Sol is now second on both IOI and SRE Bench, so the scientific gap has narrowed.
- **Sonnet 5.5 is the most cautious model in the field and the least accurate when decisive.** It declines or qualifies nearly half of knowledge questions, posting the highest non-hallucination rate tested at 52.99%. But GPT-6.1 Sol is right eight points more often, and Fable 5.1 thirteen points more often, on the questions they attempt. In regulated work that gap decides the model.
- **Sol is the cost floor, and the gap widened.** GPT-6.1 Sol costs $0.72 per task against Sonnet 5.5’s $7.60, and it halved the cached-input rate to $0.10 on the way in. Sonnet 5.5 is now the most expensive model per task in this comparison.
- **Both labs’ newest models retreat from security work.** GPT-6.1 Sol scores 39.29% on CyberBench, last in the field and 35 points below its own predecessor. Sonnet 5.5 is mid-table at 59.58%. Neither vendor’s newest mid-tier is the one to pick for vulnerability research.

Capability per dollar, in other words, is highest in the middle of this field, not at either end. The models that look most impressive on a leaderboard are frequently among the worse buys.

Every figure is as published by the providers in late September 2026.

| Specification | Claude Sonnet 5.5 | GPT-6.1 Sol | Claude Opus 5.5 | Claude Fable 5.1 | GPT-6 Astra | Claude Opus 5 | Claude Sonnet 5 | 
|---|---|---|---|---|---|---|---|
| Release date | Sep 28, 2026 | Late Sep 2026 | Sep 22, 2026 | Sep 1, 2026 | Sep 3, 2026 | Jul 2026 | Jun 2026 | 
| Input, per 1M tokens | $2.00 | $2.00 | $4.00 | $10.00 | $10.00 | $5.00 | $2.00 | 
| Output, per 1M tokens | $10.00 | $10.00 | $20.00 | $50.00 | $50.00 | $25.00 | $10.00 | 
| Cached input, per 1M | $0.20 | **$0.10** | $0.20 | $0.25 | $1.00 | $0.50 | $0.20 | 
| Cache write, 5-minute TTL | $2.50 | $2.50 | $5.00 | $12.50 | $12.50 | $6.25 | $2.50 | 
| Cache write, 1-hour TTL | $4.00 | $2.50 | $8.00 | $20.00 | $12.50 | $10.00 | $4.00 | 
| Context window | 1M tokens | 1M tokens | 1M tokens | 1M tokens | 1.05M tokens | 1M tokens | 1M tokens | 
| Max output tokens | 128k | 128k | 128k | 128k | 128k | 128k | 128k | 
| Knowledge cutoff | June 2026 | April 2026 | June 2026 | June 2026 | April 2026 | May 2026 | January 2026 | 
| Input modalities | Text, image, file | Text, image | Text, image | Text, image | Text, image | Text, image | Text, image | 
| Long-context surcharge | None, full 1M at standard rates | 2x input, 1.5x output above 272K | None, full 1M at standard rates | None, full 1M at standard rates | 2x input, 1.5x output above 272K | None, full 1M at standard rates | None, full 1M at standard rates | 
| AA Intelligence Index, max | 56 | 52 | 58 | 53 | 53 | 51 | 38 | 
| AA cost per task, max | $7.60 | **$0.72** | $5.98 | $7.63 | $3.26 | $5.86 | $5.09 | 
| Output speed | **139 t/s** | 69 t/s | 94 t/s | 68 t/s | 63 t/s | 60 t/s | 81 t/s | 
| Vals Index | 69.22% | 61.16% | **69.69%** | 68.83% | 66.61% | 67.21% | 59.61% | 

Read the second and third rows and the two are still neck and neck on the headline rates. The parity stops there. GPT-6.1 Sol charges half as much for a cached read, which is the dominant cost line in agentic coding, and it carries no option to pay more for a longer-lived cache. On long inputs Sonnet 5.5 bills $2 and $10 for a prompt of any size up to its full 1M-token window, while GPT-6.1 Sol re-bills the entire request at $4 and $15 once it crosses 272K input tokens, including the first 272K. OpenAI applies the same rule to Astra at $20 and $75.

The cache-write TTLs are the other quiet difference. Anthropic charges 1.25x input for a five-minute cache and 2x for an hour, so Sonnet 5.5 costs $2.50 or $4.00 depending on which you choose. OpenAI charges $2.50 either way, with no hour-long option. In agentic coding, where most input tokens are cache reads, that choice decides whether you re-write the cache after every short break or keep it warm, and on Sonnet 5.5 the longer cache costs 60% more.

And underneath all of it sits the trap. Rate cards are per token, and token consumption is a behavioral property of a model, not a published specification. Sonnet 5.5 and GPT-6.1 Sol share an input and output price and differ more than tenfold in what a task actually costs.

This is the full head-to-head, before the cost is taken into account. Artificial Analysis runs eleven evaluations at matched effort levels; Vals AI runs agentic workflows with file systems, spreadsheets and documents and scores whether the job got finished. They agree on the direction and differ on the margins.

| Artificial Analysis evaluation | Claude Sonnet 5.5 | GPT-6.1 Sol | Winner | 
|---|---|---|---|
| AA-Briefcase v1.1 | **66%** | 53% | Sonnet 5.5 | 
| GDPval-AA v2.1 | **67%** | 54% | Sonnet 5.5 | 
| AutomationBench-AA | **71.3%** | 66.6% | Sonnet 5.5 | 
| Terminal-Bench 4.0 | **63.6%** | 56.1% | Sonnet 5.5 | 
| SciCode | **61.0%** | 54.2% | Sonnet 5.5 | 
| Humanity’s Last Exam | **55.0%** | 52.9% | Sonnet 5.5 | 
| AA-Omniscience non-hallucination | **52.99%** | 45.73% | Sonnet 5.5 | 
| AA-LCR v1.1 (long context) | 82.7% | **83.0%** | GPT-6.1 Sol | 
| CritPt | 31.4% | **31.7%** | GPT-6.1 Sol | 
| GDP.pdf | 25.8% | **32.0%** | GPT-6.1 Sol | 
| AA-Omniscience accuracy | 53.95% | **62.0%** | GPT-6.1 Sol | 
| Intelligence Index composite | **56** | 52 | Sonnet 5.5 | 
| Cost per task | $7.60 | **$0.72** | GPT-6.1 Sol | 

Sonnet 5.5 takes seven of the eleven evaluations and loses four. Three of those losses are narrow: long-context reasoning by three tenths, CritPt by three tenths, GDP.pdf by six points. The fourth, knowledge accuracy, is not close.

| Vals AI benchmark | Claude Sonnet 5.5 | GPT-6.1 Sol | Winner | 
|---|---|---|---|
| Vals Index (composite) | **69.22%** | 61.16% | Sonnet 5.5 | 
| Vibe Code Bench v1.1 | **92.39%** | 88.93% | Sonnet 5.5 | 
| MedScribe | **91.10%** | 86.45% | Sonnet 5.5 | 
| BioMysteryBench | **81.11%** | 79.63% | Sonnet 5.5 | 
| EMB (Excel Modelling) | **75.71%** | 70.77% | Sonnet 5.5 | 
| Tax Agent Bench | **73.39%** | 62.31% | Sonnet 5.5 | 
| Code Migration | **69.83%** | 65.12% | Sonnet 5.5 | 
| Public Benefits Bench | **67.19%** | 59.34% | Sonnet 5.5 | 
| CyberBench v1.1 | **59.58%** | 39.29% | Sonnet 5.5 | 
| Finance Agent v2 | **58.10%** | 52.03% | Sonnet 5.5 | 
| MedCode | **52.92%** | 48.84% | Sonnet 5.5 | 
| SAGE | **51.77%** | 46.54% | Sonnet 5.5 | 
| MysteryMechanism | **49.10%** | 46.40% | Sonnet 5.5 | 
| Legal Research Bench | **48.08%** | 38.46% | Sonnet 5.5 | 
| ProofBench v1.1 | **100.00%** | 99.00% | Sonnet 5.5 | 
| IOI | 83.06% | **96.89%** | GPT-6.1 Sol | 
| Terminal-Bench 4.0 | 53.03% | **55.05%** | GPT-6.1 Sol | 
| SRE Bench | 30.15% | **50.76%** | GPT-6.1 Sol | 
| Harvey’s Legal Agent | 2.92% | **5.42%** | GPT-6.1 Sol | 

Sonnet 5.5 takes fifteen of nineteen on the Vals suite. The four losses are not scattered: three of them sit in algorithmic and systems work, where GPT-6.1 Sol is second in the field on both IOI and SRE Bench behind only GPT-6 Astra.

**Which is where cost enters, and it inverts the result.** Sonnet 5.5 is the heaviest token consumer Artificial Analysis has ever measured, at roughly 193,000 output tokens per index task. At a shared output price, that single behavioral difference accounts for most of the $7.60 versus $0.72 gap. The model that won twenty-two of the thirty benchmark rows it can be scored on loses decisively on the bill.

One important caveat on that cost figure. It holds at maximum effort. Sonnet 5.5 costs between $0.41 and $7.60 per task depending on the setting, an eighteen-fold spread on one rate card, so any honest cost comparison has to name the effort level. Every benchmark figure in this article is measured at maximum effort.

Two models priced at $10 input and $50 output per million tokens were the reference points most buyers were comparing against three months ago. At a fifth of their token price, Sonnet 5.5’s composite win reads as a rout. The per-benchmark detail is where the useful information is.

| Vals AI benchmark | Claude Sonnet 5.5 | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|
| Vals Index (composite) | 69.22% | 66.61% | 68.83% | **69.69%** | 
| Vals RSI Index | Not published | 27.07% | 36.09% | **37.31%** | 
| Code Migration | **69.83%** | 67.74% | 54.61% | 66.65% | 
| Vibe Code Bench v1.1 | **92.39%** | 89.59% | 90.26% | 90.29% | 
| ProgramBench | Not published | 5.50% | 7.00% | **18.50%** | 
| Terminal-Bench 4.0 | 53.03% | 57.07% | 49.49% | **61.62%** | 
| Terminal-Bench Science | 38.57% | **65.71%** | 34.29% | 48.57% | 
| IOI | 83.06% | **100.00%** | 90.78% | 95.06% | 
| SRE Bench | 30.15% | **56.87%** | 22.90% | 33.59% | 
| MysteryMechanism | 49.10% | **53.15%** | 47.75% | 49.55% | 
| ProofBench v1.1 | 100.00% | 99.00% | 100.00% | 100.00% | 
| BioMysteryBench | **81.11%** | 79.26% | Not published | 79.26% | 
| Finance Agent v2 | 58.10% | 53.54% | **58.88%** | 58.59% | 
| EMB (Excel Modelling) | 75.71% | 71.70% | **76.67%** | 75.94% | 
| Tax Agent Bench | 73.39% | 63.34% | **77.64%** | 70.50% | 
| Legal Research Bench | 48.08% | 39.42% | **55.29%** | 50.48% | 
| Public Benefits Bench | 67.19% | Not published | **74.90%** | 70.64% | 
| MedScribe | 91.10% | 87.91% | 91.29% | **91.43%** | 
| MedCode | 52.92% | 48.49% | **53.51%** | 49.80% | 
| CyberBench v1.1 | 59.58% | 41.07% | **70.42%** | 55.36% | 
| Harvey’s Legal Agent | 2.92% | 5.42% | **6.67%** | 3.75% | 
| CUA-bench | Not published | **19.17%** | 13.17% | 14.00% | 
| Time Horizon Index | Not published | **90.50%** | 63.33% | Not published | 

Counting only rows where all four models have a published score, Sonnet 5.5 beats GPT-6 Astra 13 to 6 and beats Claude Fable 5.1 by only 8 to 10 with one tie.

**That second number is the one to sit with.** Sonnet 5.5 wins the composite index against its own sibling by four tenths of a point and loses the head-to-head. The composite win is carried by code migration and terminal work, not by the professional knowledge tasks Fable 5.1 is bought for.

Astra is a different shape of defeat. It wins the scientific and systems end of the table outright, by margins that should not be dismissed. If your work is research-adjacent, Astra at $10 and $50 remains the better buy, and switching to Sonnet 5.5 would be a downgrade in exactly the categories where you needed the capability. OpenAI has since replaced both GPT-6 tiers with GPT-6.2: see our GPT-6.2 Astra vs Sol comparison for the new 62 vs 57 Intelligence Index scores and which model to pick. That tier is also being challenged from below: Google’s Gemini 4 Argon vs GPT-6 Astra comparison has Argon matching Astra’s headline intelligence score at a fifth of the per-token price.

The pattern is consistent across every comparison in this article. Sonnet 5.5 wins on code, and loses or ties on the long professional and scientific tail. That is a coherent specialization, not a general improvement, and it is worth pricing accordingly.

For context on where the professional tier still stands, Fable 5.1 also holds the strongest scores available on the older academic suite: 93.43% on GPQA Diamond, 90.52% on LiveCodeBench, 88.51% on LegalBench, 92.38% on MMLU Pro and 90.64% on MMMU Pro. Those benchmarks are not published for Sonnet 5.5, so no direct comparison is possible, but they explain why the professional tier has not been obsoleted.

The composite scores hide more than they reveal. Artificial Analysis runs eleven individual evaluations, ten of which feed the Intelligence Index, and they break the pattern the composites suggest.

| Artificial Analysis evaluation | Claude Sonnet 5.5 | Claude Opus 5.5 | GPT-6 Astra | Claude Fable 5.1 | GPT-6.1 Sol | 
|---|---|---|---|---|---|
| AA-Briefcase v1.1 | 66% | 66% | 53% | 59% | 53% | 
| GDPval-AA v2.1 | 67% | 67% | 52% | 62% | 54% | 
| AutomationBench-AA | **71.3%** | 69.5% | 68.5% | 59.4% | 66.6% | 
| Terminal-Bench 4.0 | **63.6%** | 59.6% | 59.1% | 52.0% | 56.1% | 
| SciCode | 61.0% | **66.9%** | 56.5% | 63.1% | 54.2% | 
| Humanity’s Last Exam | 55.0% | **61.4%** | 54.7% | 59.1% | 52.9% | 
| GDP.pdf | 25.8% | 26.2% | 31.0% | 26.2% | **32.0%** | 
| CritPt | 31.4% | 31.7% | 31.7% | 29.7% | **31.7%** | 
| AA-Omniscience accuracy | 53.95% | 66.22% | 62.60% | **67.23%** | 62.0% | 
| AA-Omniscience non-hallucination | **52.99%** | 41.39% | 48.66% | 27.42% | 45.73% | 
| AA-LCR v1.1 (long context) | 82.7% | 84.7% | 80.7% | **85.3%** | 83.0% | 
| Terminal-Bench-Science 0.1* | 53.3% | 59% | **63.3%** | 43.3% | 58.1% | 
| MLCR-AA* | **75.0%** | 66.7% | 35% | 71.1% | 33.9% | 
| Harvey LAB-AA (criterion pass rate)* | **93.1%** | 91.2% | Not published | 93.0% | Not published | 

**Sonnet 5.5 is the least accurate model in the field when it commits to an answer, and the least likely to be wrong.** Those two statements are not in conflict, and the table is how you see why.

On AA-Omniscience accuracy Sonnet 5.5 scores 53.95%, the lowest of the five, against 67.23% for Fable 5.1, 66.22% for Opus 5.5, and 62% for both GPT-6.1 Sol and Astra. On the non-hallucination rate it posts the highest figure in the comparison at 52.99%, against 48.66% for Astra, 45.73% for GPT-6.1 Sol, and 41.39% for Opus 5.5. Claude Fable 5.1 posts the lowest non-hallucination rate of all, at 27.42%, despite holding the highest accuracy.

So the Claude models sit at opposite ends of the same trade. Fable 5.1 answers more and is right more when it answers. Sonnet 5.5 declines or qualifies nearly half of knowledge questions, but when it does commit it is the least likely model here to be hallucinating. GPT-6.1 Sol is the clearest illustration of the other approach: 62% accuracy and a 54.27% hallucination rate, because it attempts far more questions than Sonnet does.

The practical consequence cuts against the usual vendor framing. Accuracy and reliability are not the same property, and the models rank in opposite orders on them. Where a declined answer is acceptable and a wrong one is expensive, Sonnet 5.5’s caution is the safer profile, and no other model in this comparison offers it. Where the answer itself has to be right, use Fable 5.1.

Three rows sit outside the eleven-evaluation Intelligence Index composite, marked with an asterisk above. MLCR-AA is worth singling out: Sonnet 5.5 leads the field at 75.0%, beating Fable 5.1 by four points and GPT-6.1 Sol by forty-one. On Harvey LAB-AA’s legal criterion pass rate it edges Fable 5.1 by a tenth of a point, with Opus 5.5 third. And on Terminal-Bench-Science 0.1, GPT-6.1 Sol’s 58.1% puts it second in the field, behind only Astra.

The rest of the table is more flattering: Sonnet 5.5 leads all five models on AutomationBench-AA and Terminal-Bench 4.0, and matches Opus 5.5 on both knowledge-work evaluations despite the fifth of the price.

Note what it does not win. SciCode and Humanity’s Last Exam both go to Opus 5.5, GDP.pdf to GPT-6.1 Sol. That headline 56 against 58 is carried entirely by coding, terminal work and business automation, and by nothing else.

Read the rows as a routing guide. Sonnet 5.5 for agentic coding and business automation, where it leads outright. Opus 5.5 for scientific coding and general reasoning. Fable 5.1 for the highest accuracy and long-context work, and for professional tasks where the answer has to be right. Astra for scientific document reasoning. GPT-6.1 Sol for high-volume extraction on a tight budget.

Artificial Analysis also publishes composite indexes for each domain, built from the evaluations above. They are the cleanest summary of where each model is strong, because no single benchmark can distort them.

| Artificial Analysis index | Claude Opus 5.5 | Claude Sonnet 5.5 | Claude Fable 5.1 | GPT-6 Astra | GPT-6.1 Sol | 
|---|---|---|---|---|---|
| Coding Agent | 66 | **68** | 62 | 62 | Not published | 
| Finance & Accounting | **61** | 57 | 56 | 55 | 54 | 
| Strategy & Ops | **64** | 60 | 60 | 57 | 57 | 
| Legal | **63** | 56 | 60 | 59 | 58 | 
| Healthcare & Medical | **61** | 58 | 58 | 52 | 51 | 
| Engineering | **60** | 58 | 56 | 55 | 54 | 
| Economics | **66** | 61 | 63 | 60 | 59 | 

Two results stand out. Sonnet 5.5 leads the Coding Agent Index outright, ahead of Opus 5.5, and it is the only domain where any model beats the flagship. That comes with a caveat worth weighing: each model was run in its own vendor’s harness, the Claude models in Claude Code and the OpenAI models in Codex, so the ranking reflects the toolchain as much as the model. GPT-6.1 Sol has not been added to this index yet.

The second is Legal, where Sonnet 5.5 finishes last of the five. It is Sonnet’s only domain ranking below third, and GPT-6.1 Sol’s entry at 58 pushed it from fourth to fifth. It points the same way as the independent legal results further up, and it is the clearest single signal that this is not a model to buy for contract review. Its head-to-head win over Fable 5.1 among the domain indexes is Engineering.

Opus 5.5 leads six of the seven, which is the clearest argument that the flagship still earns its price despite costing twice as much per token.

Claude Sonnet 5.5 finishes 28th of 41 models on CyberBench, at 59.58% or 41.97% depending on how its fallbacks are counted. That is mid-table. The bottom of the table is the more interesting result.

GPT-6.1 Sol scores **39.29%**, last of the six models compared here, and a 35-point collapse from GPT-6 Sol’s 74.58% on the same benchmark. The mid-tier OpenAI model did not merely fail to improve on offensive security work. It retreated sharply from it, which is the opposite of what happened on every other benchmark in this article. The field now reads: Anthropic’s restricted Opus 5.5 CVP at 74.58%, Claude Fable 5.1 at 70.42%, Sonnet 5.5 at 59.58%, Opus 5.5 at 55.36%, GPT-6 Astra at 41.07%, and GPT-6.1 Sol last at 39.29%.

The explanation for Sonnet’s own position is in Vals AI’s fallback data, and it is the most useful piece of analysis in this comparison. Sonnet 5.5 refused 2.27% of tasks, falling back to Claude Sonnet 5 server-side. Counting those fallback-assisted tasks as failures changes the numbers sharply:

| Benchmark | With fallback | Fallback counted as failure | Change | 
|---|---|---|---|
| Vals Index | 69.22% | 68.89% | -0.33 pts | 
| Terminal-Bench 2.1 | 83.15% | 80.52% | -2.63 pts | 
| Terminal-Bench 4.0 | 53.03% | 50.51% | -2.52 pts | 
| SRE Bench | 30.15% | 19.08% | -11.07 pts | 
| CyberBench v1.1 | 59.58% | 41.97% | -17.61 pts | 

On the composite index the effect is negligible. On security work it is catastrophic. CyberBench drops by nearly 18 points and SRE Bench by 11, because those are exactly the categories where a safety-tuned model declines, and a declined task in a real security workflow is a failure whether or not a weaker model quietly handled it in the background.

**The safety tax is not an Anthropic phenomenon.** Both labs’ newest mid-tier models pull back from security work, and OpenAI’s regression is the larger of the two. What differs is disclosure. Anthropic publishes its fallback rates, so we can quantify exactly how much of Sonnet 5.5’s 59.58% is earned by the model it defers to. OpenAI publishes nothing equivalent, so GPT-6.1 Sol’s fall is visible in the score and invisible in the mechanism.

There is one result that complicates the picture, and it is worth stating rather than smoothing over. On SRE Bench, which tests reverse-engineering binaries without source, GPT-6.1 Sol scores **50.76%** and finishes second in the field behind Astra. So it is not declining security work broadly. A model that ranks second on binary reverse engineering and last on vulnerability finding is drawing a policy line somewhere specific, and Vals AI does not publish which tasks it declined, so the boundary is not yet visible. Anyone whose work sits near that line should measure it directly rather than infer it from either score.

Anthropic’s top score on CyberBench comes from a configuration most people cannot buy. **CVP** is Anthropic’s Cyber Verification Program, a restricted mode with security safeguards reduced, available only to approved organizations. It is a valid measurement of what the underlying model can do when allowed to, and Opus 5.5 CVP scores 74.58% against the standard Opus 5.5’s 55.36%.

Set the CVP aside and Anthropic’s strongest generally available score is Fable 5.1 at 70.42%, still well clear of Sonnet 5.5. But the caveat cuts both ways. The CVP variant falls back on 42.86% of tasks once fallbacks are counted as failures, a larger penalty than Sonnet 5.5’s 2.27%, which suggests fallback behaviour is configured per variant rather than set once across Anthropic’s line.

There is also a reason the gate exists, and it is worth knowing before treating refusals as excessive caution. On Anthropic’s own ExploitBench evaluation, Sonnet 5.5 achieved full arbitrary code execution in **43.4% of runs**. Sonnet 5 never built a working Firefox exploit in the equivalent June evaluation. The model is substantially more capable at offensive security work than its predecessor, which is precisely why it ships with the same three-stage cyber gate as Opus 5.5.

This is the table that should drive a purchasing decision, and its ordering is not monotonic.

| Model at maximum effort | Intelligence Index | Cost per task | Output speed | Input per 1M | Output per 1M | 
|---|---|---|---|---|---|
| Claude Opus 5.5 | 58 | $5.98 | 94 t/s | $4.00 | $20.00 | 
| **Claude Sonnet 5.5** | 56 | $7.60 | **139 t/s** | $2.00 | $10.00 | 
| Claude Fable 5.1 | 53 | $7.63 | 68 t/s | $10.00 | $50.00 | 
| GPT-6 Astra | 53 | $3.26 | 63 t/s | $10.00 | $50.00 | 
| GPT-6.1 Sol | 52 | **$0.72** | 69 t/s | $2.00 | $10.00 | 
| Claude Opus 5 | 51 | $5.86 | 60 t/s | $5.00 | $25.00 | 
| Claude Sonnet 5 | 38 | $5.09 | 81 t/s | $2.00 | $10.00 | 

Sonnet 5.5 is the second most capable model in the field and the most expensive one to run at maximum effort, which is counterintuitive given it costs a fifth as much per token as Fable 5.1 and half as much as Opus 5.5. The reason is verbosity: at roughly 193,000 output tokens per index task it generates more tokens than any other frontier model Artificial Analysis has measured, so the lower unit price does not produce a lower bill.

GPT-6.1 Sol shows the opposite profile. It scores four points below Sonnet 5.5 and costs ten times less per task, and it improved on its predecessor in both directions at once: GPT-6 Sol scored 48 at $1.06 per task, where GPT-6.1 Sol scores 52 at $0.72. Cheaper and smarter, which is rare.

Capability per dollar is highest at the bottom of this table, not the top. The least capable current model is the cheapest by a wide margin, the most capable is beaten on cost by three others, and the model most people would assume is the value pick on unit price is the most expensive to actually use.

That ranking is specific to maximum effort and to the benchmark composite. Both caveats matter: Sonnet 5.5’s per-task cost falls by an order of magnitude at lower effort settings, and its 139 tokens per second is twice GPT-6.1 Sol’s rate, which a per-task figure does not capture at all.

Anyone deploying this without pinning the effort level is flying blind, because the default varies by surface and the bill follows whatever it happens to be.

Look past the individual benchmark rows and a clearer picture emerges. Anthropic holds the top of the Artificial Analysis Intelligence Index, and it holds the top two places. Claude Opus 5.5 leads at 58, Sonnet 5.5 is second at 56, and then Fable 5.1 and GPT-6 Astra tie at 53 with GPT-6.1 Sol one point behind. The same ordering holds on the Vals Index: Opus 5.5 at 69.69%, Sonnet 5.5 at 69.22%, Fable 5.1 at 68.83%, Astra at 66.61%, and GPT-6.1 Sol at 61.16%.

That is the opposite of the position a year ago, and it is the result of a deliberate pricing strategy rather than a single breakthrough. Anthropic built a four-rung ladder and is selling down it. Fable 5.1 and Mythos 5.1 sit at $10 and $50 for the hardest reasoning. Opus 5.5 dropped to $4 and $20 when it launched. Sonnet 5.5 launched at $2 and $10. And Anthropic has confirmed that **Haiku 5.5 will join the family in the coming weeks** as the smallest and cheapest model, which on the pattern so far should land well below $2 and $10.

OpenAI’s response has been to iterate fast and cut hard. GPT-6.1 Sol arrived within hours of GPT-6 Sol at the same headline rates but with cached input halved to $0.10, a better score, and a cost per task down from $1.06 to $0.72. That is a real answer to Anthropic on price, and this time it came with a capability gain rather than a trade.

Which is why Haiku 5.5 matters more than it looks. OpenAI’s cheapest serious model costs $2 and $10 with a $0.10 cached rate. If Anthropic drops a capable model beneath that, the bottom of the market compresses again and the GPT-6 tier loses the clearest advantage it has. A high-volume extraction pipeline priced against GPT-6.1 Sol today is priced against a different market in six weeks’ time.

Sonnet 5.5 is expensive per task, and the honest reason is verbosity. At roughly 193,000 output tokens per index task it is the heaviest token consumer Artificial Analysis has measured, which is why a cheaper model on paper produces a larger bill. That much is a genuine drawback.

But verbosity and latency are not the same thing, and Sonnet 5.5 has an advantage the cost tables do not capture. At 139 tokens per second it is the fastest model in this comparison, just over twice GPT-6.1 Sol’s 69, and more than twice Fable 5.1’s 68. GPT-6.1 Sol also slowed down: its predecessor ran at 85 tokens per second.

The ordering tracks model size rather than price. Sonnet 5.5 is a smaller model than Fable 5.1 or Opus 5.5, and smaller models produce tokens faster, so the reasoning that makes it expensive per task is also the reasoning that makes it quick to stream.

That matters for two reasons. For interactive work, an agent that emits at 139 tokens per second finishes a long chain of thought considerably sooner than one running at 69, and the wall-clock experience of a model is not the same thing as its invoice. For high-volume batch work, throughput is what determines wall-clock cost, and Sonnet 5.5’s token rate means a task completes in roughly half the time of the same task on Fable 5.1 at a similar per-task price.

So the accurate summary of Sonnet 5.5 is narrower than “expensive.” It is the most verbose model in the field and the fastest. Whether that is a good trade depends entirely on whether you are paying for tokens or for elapsed time, and nobody’s benchmark reports the second.

Two predictions, offered as analysis rather than fact.

**Fable 5.5 is likely, and it would probably take the top spot.** Fable 5.1 was Anthropic’s most capable model before Opus 5.5 overtook it, and each release in this family has pushed the ceiling higher. A Fable 5.5 would be expected to score above 58 and reclaim the lead, at a higher price per token than Opus 5.5 rather than lower. Anthropic has not confirmed it, and the gap between a prediction and a launch is wide, but the pattern across the 5 and 5.1 generations makes it the most probable next move.

**Someone else may take the top spot first.** OpenAI moved from GPT-6 Sol to GPT-6.1 Sol in a matter of hours, improving both score and cost per task. This field has moved at least three index points in a month, and a faster cadence at the top of the OpenAI lineup makes the ceiling less stable, not more. Assuming Anthropic’s lead holds until Fable 5.5 arrives is a bet, and the recent record suggests it is not a safe one.

The durable point is that the frontier is no longer ordered by vendor. The top four positions on the index currently hold three Anthropic models and one OpenAI model, at four different price points, and the cheapest model in the field is nowhere near the worst. Choosing a model means choosing which kind of work it is for, not which laboratory built it. Meta has since joined the top of that table: Muse Wonder Full lands joint second at 56, level with Claude Sonnet 5.5. Since then the lead has moved again, and our Fable 5.2 vs Gemini Carbon comparison breaks down the current top two on Vals and Artificial Analysis.

Anthropic’s Sonnet 5.5 announcement is unusually transparent in one respect: it publishes Sonnet 5’s scores alongside, and some of those numbers are unflattering.

| Benchmark | Claude Sonnet 5.5 | Claude Sonnet 5 | Claude Opus 5.5 | GPT-6 Sol | 
|---|---|---|---|---|
| Terminal-Bench 4.0 | 70.6% | 10.3% | 66.4% (xhigh) | Not disclosed | 
| FrontierCode 1.1 (main), max | 46.2% | 42.4% | 54.4% | 49.3% | 
| FrontierCode 1.1 (main), xhigh | 52.1% | 42.4% | 54.4% | 49.3% | 
| CursorBench 4.0 | 55.5% | 34.1% | 57.8% | Not disclosed | 
| GDPval-AA v2.1 (Elo) | 1844 | 1449 | 1846 | 1487 | 
| AA-Briefcase v1.1 (Elo) | 1811 | 1359 | 1822 | 1483 | 
| Humanity’s Last Exam, with tools | 64.5% | 54.9% | 67.7% | Not disclosed | 
| OSWorld 2.1 (partial credit) | 80.1% | 57.0% | 81.8% | Not disclosed | 
| Chartography (no tools) | 61.6% | 15.6% | 64.4% | Not disclosed | 

Note that Anthropic’s table compares against GPT-6 Sol, published before GPT-6.1 Sol existed. Against the current OpenAI mid-tier the picture would be narrower on knowledge work and unchanged on the coding rows Anthropic chose not to disclose for either Sol.

Anthropic’s framing is that Sonnet 5.5 essentially ties Opus 5.5 on knowledge work, and the GDPval and AA-Briefcase rows bear that out.

On frontier code generation Sonnet 5.5 actually trails the Sol line at matched effort, 46.2% against 49.3%, and only passes it at xhigh effort with 52.1%. That is worth pausing on, because it runs against both independent suites, where Sonnet 5.5 beats GPT-6.1 Sol on Terminal-Bench 4.0, AutomationBench, Code Migration and Vibe Code Bench alike. Different harnesses produce different answers on agentic coding, which is the least standardized measurement in the field.

The Terminal-Bench 4.0 jump from 10.3% to 70.6% against Sonnet 5 sounds like an order of magnitude, and it is, but it should be read as repairing a near-total failure rather than a normal increment. Anthropic also notes a standard error of roughly plus or minus 2.6 points on that benchmark, so 70.6% against Opus 5.5’s 66.4% is not a meaningful lead.

Sonnet 5.5 is also the first Sonnet model to complete Pokémon Red working only from screenshots, a detail that says more about long-horizon agentic persistence than any single benchmark row.

Note what Anthropic does not report: no competitor figure for Terminal-Bench 4.0, CursorBench 4.0 or Humanity’s Last Exam. When a vendor omits a competitor’s row from their own table, treat it as a competitive loss.

OpenAI’s framing is cost per task rather than raw scores, which is the right frame given what Artificial Analysis measured. The earliest published figures below are from GPT-6 Sol and predate GPT-6.1 Sol.

- **Agents Last Exam:** Sol at maximum effort scores 56.4%, which OpenAI says beats the best Opus 5 result in that evaluation at 60% lower cost.
- **AutomationBench:** Sol at xhigh effort scores 33.2% at $0.27 per task, above Opus 5 at maximum effort.
- **DeepSWE v1.1:** Sol scores 68.8%, close to Claude Fable 5 and Opus 5 levels at 80% to 96% lower cost per task.
- **Hallucination rate:** Artificial Analysis measured the Sol line’s AA-Omniscience hallucination rate falling from 92% to 60% on GPT-6 Sol, achieved by answering fewer questions. Accuracy dropped from 59% to 54% as a result, which was a trade rather than a win. GPT-6.1 Sol reverses it: accuracy back up to 62%, hallucination rate at 54.27%.

Independent testing broadly supports the cost framing while adding context. Sol’s AutomationBench-AA rose from 61.6% on GPT-6 Sol to 66.6% on GPT-6.1 Sol, with Terminal-Bench 4.0 climbing from 43.9% to 56.1% across the same two releases. Vals AI shows the same direction, with Code Migration up from 57.20% to 65.12% and Vibe Code Bench up from 87.82% to 88.93%. The one benchmark that moved sharply backwards is CyberBench, down from 74.58% to 39.29%.

Vals AI and Artificial Analysis are both credible, and they emphasize different things. Understanding why is more useful than picking a winner.

- **Different tasks.** Vals AI runs full agentic workflows with file systems, spreadsheets and documents, and scores whether the job got finished. Artificial Analysis runs a composite of scored evaluations, many of which reward reaching a correct answer after extended deliberation.
- **Different safeguards, and different disclosure.** Vals AI runs Anthropic’s production safeguards and counts refusals, and Anthropic publishes the resulting fallback rates. OpenAI publishes no equivalent, so the same effect is visible in the scores and invisible in the mechanism.
- **Different coverage of the newest models.** Artificial Analysis has published GPT-6.1 Sol across its full evaluation set but has not yet added it to the Coding Agent Index. Vals AI has the benchmark scores but not the token-usage or latency data.
- **Index versions.** Artificial Analysis has rebuilt its Intelligence Index several times. A 56 from v4.3.2 is not comparable to a 61 from an older build, and several of our own earlier articles quote the older scale. Always read the version number, not just the score.

Where both evaluators agree is the useful finding. Both place Sonnet 5.5 far ahead of Sonnet 5, both place it within two points of Opus 5.5 on aggregate capability, and both place the Sol line in a middle band: behind the Claude 5.5 models on intelligence, but with a cost profile none of them match.

It depends on your per-task budget, your latency tolerance, and how much a wrong answer costs you. Read the evaluations above as a routing guide and the reasoning mostly follows from it, but a few things are worth stating plainly.

**Sonnet 5.5 is the default for most technical work**, specifically agentic coding, business automation and anything latency-sensitive. It leads all five models on AutomationBench-AA and Terminal-Bench 4.0, takes first place on three Vals leaderboards including Code Migration and Vibe Code Bench, and matches Opus 5.5 exactly on both knowledge-work evaluations at a fifth of the per-token price. At 139 tokens per second it is also the fastest model in the field, which is decisive for anything interactive. Anthropic’s own guidance lands in the same place: its model docs position Sonnet 5.5 as “the best combination of speed and intelligence” and describe it as “a faster, lower-cost complement to Opus 5.5.”

**Choose Claude Fable 5.1** for legal, tax, medical and financial work where accuracy on attempted answers is the binding constraint. It wins Tax Agent Bench, Legal Research and MedCode, and posts the highest knowledge-accuracy score in the field at 67.23%.

**Choose Claude Sonnet 5.5** where a declined answer is acceptable and a wrong one is not. Its 52.99% non-hallucination rate is the highest tested, and on long-context medical reasoning it leads the field outright at 75.0% on MLCR-AA. The trade is explicit: eight points less accuracy than GPT-6.1 Sol, far fewer confident errors.

**Choose GPT-6.1 Sol** if cost per task dominates, or if your work is algorithmic. At $0.72 it is ten times cheaper to run than Sonnet 5.5 while scoring four index points lower, it finishes second in the field on both IOI and SRE Bench, and it leads on GDP.pdf. For high-volume extraction and competitive programming this is the obvious pick, and the margin widened with the 6.1 release rather than narrowing.

**Opus 5.5 earns its price only when you need the ceiling.** It is the only model that beats Sonnet 5.5 on the composite, and it is cheaper to run per task despite costing twice as much per token, because it reasons less. If those two index points do not change your output, Sonnet 5.5 is the better buy.

**Stay on Sonnet 5** if your tasks are not frontier-difficulty. It scores 38 against Sonnet 5.5’s 56 at the same per-token price, so the decision is purely whether you can afford the extra tokens the smarter model will use.

**Do not buy either newest mid-tier model for offensive security work** without measuring it yourself. Sonnet 5.5 drops from 59.58% to 41.97% on CyberBench once its refusals are counted as failures, and GPT-6.1 Sol posts the lowest score in the field at 39.29%.

**One operational habit matters more than the choice.** Pin the reasoning effort level on anything. Sonnet 5.5 costs between $0.41 and $7.60 per task depending on the setting, an eighteen-fold spread on a single rate card, and the default is not consistent across surfaces: medium in the Claude apps and Claude Code, high on the Claude Platform. Deploying without specifying it means your bill is decided by whichever endpoint served the request.

On measured capability, yes, but by less than before and less across the board. Sonnet 5.5 scores 56 on the Artificial Analysis Intelligence Index against 52, wins seven of eleven evaluations, and takes fifteen of nineteen Vals AI benchmarks. GPT-6.1 Sol wins IOI, SRE Bench, Terminal-Bench 4.0 and Harvey’s Legal Agent. On cost per task GPT-6.1 Sol wins by roughly ten times, at $0.72 against $7.60. On knowledge accuracy it leads by eight points; on non-hallucination rate Sonnet 5.5 leads by seven.

Yes, on most axes. It scores 52 on the Intelligence Index against 48, costs $0.72 per task against $1.06, and halved the cached-input rate to $0.10 per million tokens while holding input and output at $2 and $10. Knowledge accuracy improved from 54.48% to 62%. The exception is security: CyberBench fell from 74.58% to 39.29%.

Input and output, yes: $2 and $10 per million tokens each. Cached input does not match, at $0.20 for Sonnet 5.5 and $0.10 for GPT-6.1 Sol. Anthropic also offers an hour-long cache at $4.00 per million for writes, while OpenAI charges $2.50 regardless of duration and offers no longer-lived option. And on long inputs past 272K tokens, GPT-6.1 Sol re-bills the whole request at $4 and $15 while Sonnet 5.5 stays at standard rates across its full window.

No. At maximum effort Sonnet 5.5 costs $7.60 per task, the most expensive in this comparison, and it is beaten on cost by GPT-6.1 Sol at $0.72, by GPT-6 Astra at $3.26, and by both Opus models. Sonnet 5.5 is the right choice at lower effort settings, not at maximum.

Not either newest mid-tier without testing. GPT-6.1 Sol is last on CyberBench at 39.29%, and Sonnet 5.5 falls from 59.58% to 41.97% when its refusals are counted as failures. Fable 5.1 is the strongest generally available option at 70.42%, and Anthropic’s restricted Opus 5.5 CVP configuration reaches 74.58%. Note that GPT-6.1 Sol nonetheless ranks second on SRE Bench at 50.76%, so the pullback is specific rather than total.

It depends what failure costs you. Its non-hallucination rate of 52.99% is the highest in the field, so where a declined answer is acceptable and a wrong one is expensive, Sonnet 5.5 is the safest model tested. Where the answer itself has to be right, use Fable 5.1, which leads every professional benchmark Sonnet 5.5 loses and is right thirteen points more often on knowledge questions. Sonnet 5.5 also ranks last of five on the Artificial Analysis Legal Index, which is the clearest signal not to use it for contract review.

Alongside Opus 5.5, Anthropic raised five-hour usage limits on Pro, Max, Team and Enterprise plans and issued a rate-limit reset usable until October 22, 2026. The model ships with Fable 5.1-class safeguards on biology and cybersecurity categories, which is the direct cause of the refusal behaviour documented above. Thinking is always on and cannot be disabled on this generation.

Three things will change this picture soon. Artificial Analysis has not published GPT-6.1 Sol on the Coding Agent Index, where Sonnet 5.5 leads the field and the result is currently uncontested. Vals AI has not published token-usage or latency data for it, so the per-task cost comparison rests on a single evaluator. And Anthropic has confirmed that Haiku 5.5 will join the family in the coming weeks as the smallest and cheapest model, which should compress the bottom of the market again.

On the top spot, the reasoning is set out in the section above. In short, a Fable 5.5 is the most probable next move from Anthropic, but OpenAI’s move from GPT-6 Sol to 6.1 Sol within hours suggests the ceiling is less stable than it was, not more.

The durable point is that the frontier is no longer ordered by vendor, and it is not ordered by price either. The top four positions on the index hold three Anthropic models and one OpenAI model at four different price points. The cheapest current model is the fifth most capable. The most capable is beaten on cost per task by three others. The model most people would assume is the value pick on unit price is the most expensive to actually run.

Capability per dollar, in other words, is highest at the bottom of this field. The models that look most impressive on a leaderboard are frequently among the worse buys, and the release that should worry Anthropic is not the one that scores highest. It is the one that scored four points better, cost a third less, and arrived hours after their own launch. The one thing OpenAI gave up to get there was security work, and the size of that retreat, from 74.58% to 39.29% on CyberBench in a single point release, is the most surprising number in this article.

For the wider market context, our LLM API pricing comparison covers per-token rates across providers, our Claude Opus 5.5 review covers the model ahead of Sonnet 5.5 on the index, and our look at why GLM and MiMo undercut both labs on cost is a useful corrective when even these tiers look steep.
