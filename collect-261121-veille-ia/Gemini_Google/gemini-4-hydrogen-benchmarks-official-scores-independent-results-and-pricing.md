---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-benchmarks-official-scores-independent-results-and-pricing
title: "Gemini 4 Hydrogen Benchmarks: Official Scores, Independent Results and Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["gemini", "google", "hydrogen", "model", "benchmarks", "sonnet", "gpt-6", "terra"]
source: https://emergent.sh/learn/gemini-4-hydrogen-benchmarks
source_anchor: ""
source_lines: [1, 161]
section: "Gemini_Google"
---
#### TL;DR

- Gemini 4 Hydrogen has ratings of 51 (high) and 54 (xhigh) in the Artificial Intelligence Analysis Index. Argon and Carbon score 50 and 60, respectively.
- Hydrogen outperforms Claude Sonnet 5.5 on seven of ten benchmarks and GPT-6.1 Terra on nine of the ten comparisons shown.
- Hydrogen runs on Terminal-Bench 4.1, OSWorld-2.1, and Terminal-Bench-Science 0.1.
- $1.50 per million input tokens and $7.50 per million output tokens (both flat for 1M tokens).
- Most of the tests were conducted by Google, so check the results on your tasks.


At the default x1 setting, Gemini 4 Hydrogen scores 51, while at xhigh it scores 54, outperforming Gemini 4 Argon, which scores 50. Google's model, also priced at $1.50 per million input tokens, was released globally on November 12, 2026. According to Google's model card, it is faster than Claude Sonnet 5.5 and GPT-6.1 Terra on seven of ten benchmarks.

This guide explains the official and independent Hydrogen benchmarks, pricing, limits, and the tests where it falls behind.

## What Gemini 4 Hydrogen is

Google's fast execution model in the Gemini 4 family is derived from Google's Gemini 4 Carbon model. Google describes it as a "day-to-day workhorse for agentic workflows." Carbon and Argon are used for deep, long-term, and long-horizon reasoning, whereas Hydrogen is used for rapid execution. It will supersede the Flash branding for this generation.

Here is a summary of the essentials from the model card:

- Inputs: Text, images, audio, and video.
- Context: A 1M unified context window. Output: Up to 64K synchronous output tokens and up to 1M via background long decode.
- The thinking levels have been expanded to Low, Medium, High (default), and a new xHigh level.
- Knowledge cutoff: March 2026.

It is available in Google AI Studio, the Gemini API, Google Search, the Google Gemini app, and Google Gemini for business. It can also be used in Google Antigravity by paid Google AI Pro and Ultra users.

## Gemini 4 Hydrogen benchmarks vs Claude Sonnet 5.5 and GPT-6.1 Terra

Google's model card reports that Sonnet 5.5 leads on the remaining three: Terminal-Bench 4.1, OSWorld-2.1, and Terminal-Bench-Science 0.1. The best score in each row is in bold.

| **Category** | **Benchmark** | **Gemini 4 Hydrogen** | **Claude Sonnet 5.5** | **GPT-6.1 Terra** | 
|---|---|---|---|---|
| Agentic coding | SWE-sweep | **13.5%** | 3.1% | 5.8% | 
| Agentic coding | Terminal-Bench 4.1 (cheating-adjusted) | 57.0% | **63.9%** | — | 
| Knowledge work | Agents' Last Exam v1.1 | **71.6%** | 60.4% | 69.8% | 
| Knowledge work | AutomationBench-AA | **76.1%** | 71.8% | 60.2% | 
| Knowledge work | Vending-Bench 2 (bank balance) | **$14,549** | $11,165 | $9,107 | 
| Computer use | OSWorld-2.1 (offline subset) | 77.6% | **83.9%** | 74.2% | 
| Multimodal | MKA (Climb WSR accuracy) | **53.4%** | 52.0% | 43.8% | 
| Multimodal | Chartography | **66.6%** | 61.6% | 63.2% | 
| Science and math | Terminal-Bench-Science 0.1 | 51.4% | **59.9%** | 58.3% | 
| Reasoning | HLE-Diamond | **58.8%** | 53.3% | 47.3% | 

Source: Google DeepMind, Gemini 4 Hydrogen model card, published November 12, 2026. Google ran these evaluations; methodology is linked on the card.

Each row represents an individual exam. The higher the percentage, the more tasks are completed correctly. Vending-Bench 2 simulates a business, so the more money you have left in your bank account, the better.

The “—” indicates that the Terminal-Bench 4.1 model does not have a GPT-6.1 Terra score listed on the model card.

The results are clear for bug repair (SWE-sweep) and business-style agent tasks. Computer usage and science-in-a-terminal are led by Sonnet 5.5.

Also Read: Claude Sonnet 5.5 pricing and GPT-6.1 Terra benchmarks for the two models Hydrogen is compared with here.

## Coding and terminal results: a big jump over Gemini 3.8 Flash

Hydrogen more than triples Gemini 3.8 Flash on Terminal-Bench 4.1, scoring 57% compared with 18.2%. Google also states that it completes tasks 2.5 times faster than 3.8 Flash at its default setting and reduces fabricated claims by over 5x. Nevertheless, Sonnet 5.5 still has a higher score at 63.9%.

Hydrogen also provides information on working with code on Google. It can make focused edits without rewriting irrelevant files and performs file reads and searches in parallel groups rather than one at a time. Hydrogen was 2x to 2.8x faster in early partner tests, while consuming 12% to 23% fewer tokens. These speed figures are not from an independent lab but from Google and its partners.

Google has joined Hydrogen with its larger models, using Orchestrator in Antigravity. Orchestrator achieved near-Carbon task success, used 24% less quota and completed tasks 19% faster than Orchestrator on Terminal-Bench 4.0, while switching between Carbon and Hydrogen within a single session. To build a lead-and-worker setup of your own, a coding agent builder lets you set the roles, the checks, and where a person reviews.

**Also Read:** Gemini 4 Hydrogen vs Gemini 3.8 Flash for the full head-to-head.

## Independent results: Artificial Analysis Intelligence Index

Artificial Analysis rates Hydrogen at xhigh 54, high 51, medium 41, and low 27. The list shows scores of 60 for Gemini 4 Carbon, 50 for Gemini 4 Argon, and 31 for Gemini 3.8 Flash. Artificial Analysis is a third-party benchmarking agency.

Source: Artificial Analysis, Gemini 4 Hydrogen release page (Nov 2026).

At high beats, Hydrogen is one point ahead of Argon. It is still 6 points below Carbon at its best settings. The thinking level will take the score from 27 at low to 54 at xhigh. Cost per task starts at $0.29 for the Low setting and varies up to 11x across the four Hydrogen settings. The higher the setting, the more accurate it is, but it is more expensive.

The index blends nine evaluations. This table explains each one.

| **Evaluation in the index** | **What it measures** | 
|---|---|
| AA-Briefcase v1.1 | Agentic knowledge work (Elo score) | 
| AutomationBench-AA | Agentic SaaS workflows | 
| Terminal-Bench 4.0 | Agentic coding and terminal use | 
| FrontierCode-AA 2.0 | Agentic software engineering in open-source repositories | 
| AA-Omniscience v1.1 | Knowledge accuracy | 
| GDP.pdf | Professional document reasoning | 
| HLE-Diamond | Reasoning and knowledge | 
| CritPt v2 (Hard Subset) | Physics reasoning | 
| Terminal-Bench-Science 0.1 | Agentic scientific research workflows in a terminal | 

Source: Artificial Analysis release page, Intelligence Index v5.0.1.

The index is a weighted blend of nine evaluations (coding, office tasks, documents, and hard science questions). No single number can pinpoint weaknesses in any one area. One interesting note: the index is based on Terminal-Bench 4.0, whereas Google's model card is based on 4.1.

**Also Read:** Gemini 4 Hydrogen alternatives for six models compared on cost per task and speed.

## Safety and reliability benchmarks

Google indicates that the number of agent errors has dropped significantly from the previous year's figures with Gemini 3.8 Flash. Google claims that Hydrogen decreases overreach by up to 81%, under-service by up to 79%, and misaligned actions and shortcutting tasks by 12x.

| **Metric** | **Google's reported result** | 
|---|---|
| Overreach in agentic tasks (doing more than asked) | Up to 81% lower than 3.8 Flash | 
| Under-service in agentic tasks (doing too little) | Up to 79% lower | 
| Misaligned actions and task shortcutting | 12x fewer | 
| Indirect prompt injection (Gray Swan IPI) | More than 2.3x more resilient | 
| Internal agent honesty evaluation | 96.7% | 
| Chain-of-thought legibility | 99.9%, zero deceptive edits across 200,000+ audited turns | 
| False refusals on HealthBench Pro | Under 0.4% | 

Source: Google blog, "Introducing Gemini 4 Hydrogen" (Nov 12, 2026). These are Google's internal evaluations.

Indirect prompt injection refers to instructions embedded in a web page or file that attempt to manipulate an agent. The model card also includes automatic safety ratings for Gemini 3.8 Flash.

| **Automated evaluation** | **Hydrogen vs 3.8 Flash** | 
|---|---|
| Text-to-text safety | +0.80 points | 
| Multilingual safety | +1.10 points | 
| Image-to-text safety | +7.00 points | 
| Unjustified refusals (lower is better) | +5.57 points | 

Source: Google DeepMind model card, automated evaluations only.

The last row is a regression. The model card claims that there are more unjustified refusals than in 3.8 Flash, particularly with multimodal prompts. These two results aren't conflicting because the HealthBench Pro figure above is from a different test. The card also notes that Hydrogen does not attain critical capability levels for CBRN, but does attain alert levels for both. The complete Safety and Security Report was still to be released, Google said.

## Gemini 4 Hydrogen API pricing

The cost of hydrogen is $1.50 per 1 million input tokens, $7.50 per 1 million output tokens, and no surcharge for long context. Google's price is a flat rate for the full 1M-token window. Artificial Analysis shows a 95% cache discount.

| **Model** | **Input / output per 1M tokens** | **Notes** | 
|---|---|---|
| Gemini 4 Hydrogen | $1.50 / $7.50 | Flat across the 1M window | 
| Gemini 4 Argon | $2 / $10 | $4 / $20 from Dec 1 | 
| Gemini 4 Carbon | $2 / $10 | $4 / $20 from Dec 1 | 
| Gemini 3.8 Flash | $0.75 / $3.75 | $1.50 / $7.50 from Jan 1, 2027 | 

Sources: Hydrogen row from the Google blog. Other rows from ImportStatic, which compiled them from Google's Gemini API pricing page on Nov 13, 2026. Confirm on Google's pricing page before you budget.

Here is an example worked out using those rates. A request with 20,000 input tokens and 3,000 output tokens costs $0.0525 on Hydrogen ($0.03 input plus $0.0225 output). Today this same request will cost $0.07 on Argon; after December 1, it will cost $0.14. Thinking tokens become a cost as output, which means the more you think, the higher your actual cost.

**Also Read:** Gemini 4 Hydrogen pricing for the full rate card, free tier, and cost per task.

## Which Gemini 4 model to choose?

Use Hydrogen for rapid coding and agent work, and use Carbon for your most challenging reasoning. Google claims teams utilize Carbon for deep architectural judgment and Hydrogen for coding, test running and verifying changes. Argon becomes a viable option where Hydrogen is inadequate, and Carbon is not fast enough.

For those running 3.8 Flash today, choose the thinking level as you wish. The default Hydrogen setting is high and could lead to increased token usage if set low. Start at medium for a fair comparison, then test high for your personal evaluation set.

Knows what the numbers mean and places limits on them before using them

Use the scores as a starting point; most of the scores were run by Google. Four limits matter:

1. Vendor-run tests: Both the model card and the blog are provided by Google. Independent numbers are limited to Artificial Analysis.
2. Version differences: According to Google, the latest version of Terminal-Bench is 4.1 (cheating-adjusted). Terminal-Bench 4.0 is used in Artificial Analysis.
3. Known weaknesses: Google reports the following: hallucinations, occasional slowness or timeouts, and higher token usage at higher thinking levels.
4. Missing speed data: Independent speed results are pending; Artificial Analysis reports 7.9 minutes per Intelligence Index task at the High setting and 13.7 minutes at xhigh.

Create a mini test set of your actual work. Compare accuracy, latency, cost and output-format consistency between two or three models.

**Also Read:** Gemini 4 Carbon vs Gemini 4 Argon for how the two frontier models differ.

## Conclusion

On the independent index, the lower-cost, faster model is on par with Argon and outperforms Sonnet 5.5 and GPT-6.1 Terra in most Google tests. It lags behind Sonnet 5.5 on terminal coding, computer use and science tasks and is lower than Carbon overall. Carefully select the thinking level, and conduct your own analysis before migration.

Even with a good model, a product still needs to exist. This happens when you take initiative by creating an application or tool for your business and incorporating your idea into it. From creating a dashboard of your choice to adding a query window for clients, you can customize everything with one click. With Emergent, you can describe what you want and build a working app without writing the full stack, because it builds full-stack applications from natural-language prompts.
