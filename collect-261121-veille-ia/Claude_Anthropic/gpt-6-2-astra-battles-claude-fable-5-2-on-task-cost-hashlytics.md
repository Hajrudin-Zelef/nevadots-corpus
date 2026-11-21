---
id: collect-261121-veille-ia/claude-anthropic/gpt-6-2-astra-battles-claude-fable-5-2-on-task-cost-hashlytics
title: "GPT-6.2 Astra Battles Claude Fable 5.2 on Task Cost » HashLytics"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "astra", "task", "index", "vals", "cost", "effort", "minutes"]
source: https://hashlytics.io/gpt-6-2-astra-battles-claude-fable-5-2-on-task-cost/
source_anchor: ""
source_lines: [1, 118]
section: "Claude_Anthropic"
---
According to benchmark data from AA, Astra averages $4.00 per task at its maximum reasoning effort, while Fable runs at $26.54 per task in default fallback mode. That makes Astra 84.9% cheaper per completed task, even though Fable edges ahead on raw capability, scoring 65 to Astra’s 62 on the Intelligence Index v5.0.1.

## Same Rate Card, Very Different Invoices

The gap comes from how many tokens each model generates, not from base pricing. At maximum reasoning across the AA suite, Fable produced 275 million output tokens to solve the evaluation problems, against 65.8 million for Astra. Independent data from the Vals Index v2.2 shows the same pattern with a narrower spread: Astra costs $10.68 per test and Fable $17.94, while Fable leads on score, 71.31% to 67.66%.

| Two independent broad evaluations; scores use different scales |  |  |  | 
|---|---|---|---|
| Evaluation | Astra 6.2 | Fable 5.2 | What it measures | 
|---|---|---|---|
| AA Intelligence Index v5.0.1, Max | 62 | 65, Default Fallback | Nine evaluations across knowledge work, coding, science and knowledge/reasoning tasks | 
| AA cost per index task, Max | $4.00 | $26.54 | Evaluator’s token-based cost accounting for that suite | 
| Vals Index v2.2 | 67.66% ± 0.94 | 71.31% ± 0.96 | Sector-weighted finance, coding, legal and tax tasks; uncertainty shown is one standard error | 
| Vals cost per test | $10.68 | $17.94 | Published aggregate for Vals’ configurations | 

Engineering teams have to weigh whether Fable’s 3.65-percentage-point lead on the broad Vals index justifies paying 68% more per call on Vals’ cost figures, or 563% more on AA’s. Published records from AA’s Fable results and OpenAI’s launch metrics supply the underlying numbers.

AA’s effort sweep shows where the money goes at each setting. Astra reaches 62 at xhigh for $2.52 per task, and moving to max adds $1.48 without lifting the score. Fable also scores 62 at high for $9.08, and xhigh costs $5.75 more for the same result. Only Fable’s max setting reaches 65, at $26.54 and 40.8 minutes per task against Astra’s 10.4 minutes. At medium effort Astra actually scores a point higher, 57 to 56, for less than half the cost.

| AA effort sweep: index score, modeled inference time and weighted cost per task |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|
| Effort | Astra index | Astra minutes/task | Astra US$/task | Fable index | Fable minutes/task | Fable US$/task | 
|---|---|---|---|---|---|---|
| Low | 46 | 1.4 | $0.66 | 54 | 5.4 | $3.10 | 
| Medium | 57 | 3.7 | $1.62 | 56 | 6.8 | $3.80 | 
| High | 60 | 4.8 | $2.07 | 62 | 14.6 | $9.08 | 
| Xhigh | 62 | 5.9 | $2.52 | 62 | 25.5 | $14.83 | 
| Max | 62 | 10.4 | $4.00 | 65 | 40.8 | $26.54 | 

## A Dead Heat in Code, a Split in Science

On repository maintenance the two land almost exactly level. FrontierCode-AA 2.0, which tests real multi-file software patches, has Astra at 49.0% at xhigh effort and Fable at 49.1% at high in default fallback mode, or 48.8% at max. Vals Terminal-Bench 4.0 run through Mini-SWE gives Fable a small edge, 65.15% ± 2.31 to Astra’s 63.13% ± 1.34, but Astra finished tests in an average of 34 minutes and 17 seconds at $4.69 each, against 55 minutes and 7 seconds at $14.56 for Fable. On Vals Vibe Code v1.1, Fable leads 93.77% to 89.90%.

Science is more mixed. Astra leads the Vals Science Index v1, 68.44% to 63.74%, driven by a 71.43% score on Terminal-Bench-Science. Fable reverses that on biological data exploration, scoring 83.70% on BioMysteryBench to Astra’s 80.74%.

| Vals scientific evaluations in its published configurations |  |  | 
|---|---|---|
| Evaluation | Astra 6.2 | Fable 5.2 | 
|---|---|---|
| Science Index v1 | 68.44% ± 2.13 | 63.74% ± 2.30 | 
| Terminal-Bench-Science, Mini-SWE | 71.43% ± 5.44 | 55.71% ± 5.98 | 
| BioMysteryBench | 80.74% ± 0.37 | 83.70% ± 0.74 | 
| MysteryMechanism | 53.15% ± 3.36 | 51.80% ± 3.36 | 

## Where Fable’s Premium Pays Off

Professional work is where Fable’s lead is clearest. In Vals’ published runs it scores higher on finance, legal and tax tasks, with the widest gaps on Finance Agent and Tax Agent Bench. Fable also posted a lower cost and shorter recorded duration on the finance run. Excel modeling is close to a tie, and Astra’s one win here, on HLAB-Vals, is the exception.

| Vals professional-work results; one standard error shown |  |  | 
|---|---|---|
| Evaluation | Astra 6.2 | Fable 5.2 | 
|---|---|---|
| Finance Agent v2.1 | 60.75% ± 2.12 | 69.47% ± 1.98 | 
| Excel Modeling v1.1 | 76.03% ± 2.24 | 77.38% ± 2.19 | 
| LegalResearchBench | 42.79% ± 3.44 | 46.15% ± 3.46 | 
| Tax Agent Bench | 21.84% ± 2.79 | 32.09% ± 3.37 | 
| HLAB-Vals 1.1 | 16.25% ± 3.08 | 5.00% ± 1.72 | 

Large prompts and cached context tilt toward Fable as well. The official Astra specifications set a hard 922,000-token input limit inside its 1,050,000-token window, which stops engineers from packing maximum context alongside full generation. Anthropic’s Claude pricing documentation describes adaptive thinking defaults that let reasoning budgets expand dynamically. On price, Astra reprices any standard request with more than 272K input tokens, while Fable keeps its normal rates at every size and charges a lower cache-read rate.

| Direct API pricing per million tokens, checked November 17, 2026 |  |  | 
|---|---|---|
| Token or service category | Astra 6.2 | Fable 5.2 | 
|---|---|---|
| Standard fresh input | $8 | $8 | 
| Standard output | $40 | $40 | 
| Cache read | $0.40 | $0.20 | 
| Short cache write | $10; 30-minute retention | $10; five-minute retention | 
| Longer cache write | $20; 12-hour retention | $16; one-hour retention | 
| Batch input / output | $4 / $20 | $4 / $20 | 
| Fast input / output | $16 / $80 | $16 / $80 | 
| Ultrafast input / output | $24 / $120 promotional; $48 / $240 from March 1, 2027 | No directly equivalent named tier verified | 
| Standard input over 272K tokens | $16 fresh input; $0.80 cache read; $60 output | $8 fresh input; $0.20 cache read; $40 output | 

Worked through at Standard rates, with the same token counts on both sides, one request costs:

- 100K fresh input and 10K output: $1.20 on Astra, $1.20 on Fable
- 500K fresh input and 10K output: $8.60 on Astra, $4.40 on Fable
- 900K fresh input and 10K output: $15.00 on Astra, $7.60 on Fable
- 100K already-cached input and 10K output: $0.44 on Astra, $0.42 on Fable

## Picking Which Model to Trial First

The benchmark gaps are narrow enough that workload fit matters more than the headline score. Each row below carries the limitation that comes with it.

| Which model deserves the first trial for your work? |  |  | 
|---|---|---|
| Your priority | Start by evaluating | Reason and limitation | 
|---|---|---|
| Broad maximum-effort capability | Fable 5.2 | Leads AA’s broad index and Vals Index in the configurations examined; the extra score can cost substantially more. | 
| Complex coding in an existing repository | Both | AA’s leading retrieved configurations are within 0.1 percentage points, with different effort settings. | 
| Scientific work in a terminal | Astra 6.2 | Leads the directly retrieved Vals Mini-SWE comparison; biological-data investigation is a counterexample favoring Fable. | 
| Interactive 3D experiences | Astra 6.2 | Leads AA-3D’s human-preference ranking; this does not measure all visual or frontend work. | 
| Financial research | Fable 5.2 | Higher Finance Agent score, lower cost and shorter recorded duration in Vals’ published run. | 
| Very large prompts or repeated cached context | Fable 5.2 | No Astra-style 272K-input pricing threshold, and a lower cache-read rate; actual token counts and reasoning use still matter. | 
| Existing Work/Codex or Claude workflows | Your incumbent first | Tool compatibility, permissions, context handling and migration work can outweigh a modest benchmark gap. | 

| Documented model specifications, checked November 17, 2026 |  |  | 
|---|---|---|
| Specification | GPT-6.2 Astra | Claude Fable 5.2 | 
|---|---|---|
| Direct API model ID | `gpt-6.2-astra` | `claude-fable-5-2` | 
| Context window | 1,050,000 tokens | 1 million tokens | 
| Maximum input | 922,000 tokens | No separate maximum-input figure verified beyond the shared context limit | 
| Maximum output | 128,000 tokens | 128K tokens | 
| Documented knowledge cutoff | May 18, 2026 | June 2026 reliable/training cutoff | 
| Native input/output | Text and image input; text output | Text and image input; text output | 
| Reasoning settings | Low, medium, high, xhigh, max; `none` rejected | Five effort levels; adaptive thinking always on; high API default | 
| Main API routes | Responses and Chat Completions; tool calling requires Responses | Messages API; partner-platform details vary | 
| Weights and parameter counts | Proprietary; parameter count undisclosed | Proprietary; parameter count undisclosed | 
| Fine-tuning | Not supported | Not verified in the sources examined | 
| Published retirement commitment | No comparable date verified here | Not retired before November 9, 2027 | 

## Hashlytics Take

Identical rate cards have stopped being a useful way to compare these models. What decides the invoice is how many reasoning tokens a model burns before it answers, and in AA’s maximum-effort run Astra used less than a quarter of the output tokens Fable did, 65.8 million against 275 million. The effort sweep adds a point that is easy to miss: both vendors sell top settings that buy nothing measurable. Astra’s max tier and Fable’s xhigh tier raise the cost per task without moving the index score, so a team that leaves either on its highest setting by default is paying for effort the benchmark could not detect. Whichever model you trial, run it at two effort levels on your own tasks and price the result per solved task, not per million tokens.

 **Follow Hashlytics on Bluesky, Facebook, LinkedIn , Telegram and  X  to Get Instant Updates**
