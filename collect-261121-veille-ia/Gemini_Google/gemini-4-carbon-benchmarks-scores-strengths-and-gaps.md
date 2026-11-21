---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-benchmarks-scores-strengths-and-gaps
title: "Gemini 4 Carbon Benchmarks: Scores, Strengths, and Gaps"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-28"]
keywords: ["carbon", "gemini", "google", "scores", "argon", "claude", "opus", "model"]
source: https://emergent.sh/learn/gemini-4-carbon-benchmarks
source_anchor: ""
source_lines: [1, 160]
section: "Gemini_Google"
---
#### TL;DR

- Gemini 4 Carbon leads Google’s comparison table on 15 of 17 benchmark rows, improving over Gemini 4 Argon on all 17 evaluations.
- It scored 60 on Artificial Analysis’s Intelligence Index v5.0.1, now ranking #4 of 74 models.
- Its standout strengths are long-context reasoning (GraphWalks: 96.2%) and cybersecurity (SEC-bench Pro: 88.2%; ExploitGym: 54.8%), though the security results were run without guardrails.
- It is not unbeaten: GPT-6 Astra leads FrontierSWE 2 (65.5% vs. Carbon’s 61.2%), while Claude Opus 5.5 leads OSWorld-2.1 (87.2% vs. Carbon’s 81.5%).
- Carbon’s launch price is $2 per 1M input tokens and $10 per 1M output tokens, with cached input at $0.10; Google has published 1 December 2026 for the introductory rate, and access is now open to all.


Most published AI model benchmarks are indeed led by Gemini 4 Carbon, but it is not the top one. Google says its best score is on 15 out of 17 benchmark rows. GPT-6 Astra and Claude Opus 5.5 each win one row. Here you will find details of the scores, the differences, the test conditions, and the implications of the numbers on your work.

## What Gemini 4 Carbon Is and When It Launched

Gemini 4 Carbon is Google’s top frontier model in the Gemini 4 family. Google announced it on Oct 28, 2026. It was announced approximately two weeks after the opening of Gemini 4 Argon on October 13.

Google developed Carbon as an engine for hard software engineering and cybersecurity tasks. It's aimed at lengthy, complex tasks. Google says teams saw it run for days on a single problem.

Access started narrow initially. Google originally narrowed Carbon down to cybersecurity defenders in its Fairwind Program. And Carbon was available only to selected Fairwind cyber defenders; the Gemini API had no Carbon model ID.

However, Google has now announced that access is available to all types of users, including consumers, developers, and enterprises. Google has recommended Argon for most knowledge- and coding-related work.

## Gemini 4 Carbon Benchmarks at a Glance

In Google’s table, Carbon beats GPT-6 Astra and Claude Opus 5.5 in most rows. The table highlights key factors, including general intelligence, coding, computer use, business tasks, long context and security.

| **Benchmark** | **Gemini 4 Carbon** | **Gemini 4 Argon** | **GPT-6 Astra** | **Claude Opus 5.5** | **What it tests** | 
|---|---|---|---|---|---|
| AA Intelligence Index v5.0.1 | 60 | 50 | 57 | 58 | Overall intelligence across nine tests | 
| Vals Index v2.2 | 70.4% | 69.2% | 65.0% | 70.2% | Finance, coding, legal, and tax work | 
| FrontierSWE 2 | 61.2% | 55.0% | 65.5% | 62.3% | Hard software engineering tasks | 
| Terminal-Bench 4.0 | 65.2% | 57.1% | 59.1% | 59.6% | Work done in a command line | 
| OSWorld-2.1 (offline subset) | 81.5% | 74.7% | 80.9% | 87.2% | Controlling a computer like a person | 
| AutomationBench | 53.9% | 51.3% | 41.4% | 42.5% | Business workflow automation | 
| GraphWalks (256k–1M tokens) | 96.2% | 84.2% | 71.8% | 66.8% | Finding facts in very long inputs | 
| SEC-bench Pro | 88.2% | 83.4% | 85.4% | Not reported | Finding security flaws | 

Scores are Google-reported, published October 28, 2026. Source: Google’s Gemini 4 Carbon announcement, as tabulated by Layer3Labs (updated October 28, 2026) and VentureBeat (October 28, 2026). The AA Index score is from Artificial Analysis. “Not reported” means Google did not publish a score.

## Where Gemini 4 Carbon Leads

Carbon's major advantages seem to be long-context reasoning, security testing, and science-style terminal work. The model is also improved over Argon in 17 rows.

The long-context result is outstanding. On GraphWalks, Carbon achieves 96.2% accuracy. Claude Opus 5.5 scores 66.8%. This is a difference of almost 30 points. The test takes 256,000 to 1 million tokens. It assesses a model's ability to trace through a large amount of text.

Agent tests are largely the same. On Agents’ Last Exam v1.1, Carbon scores 77.4%, against 73.1% for GPT-6 Astra and 71.6% for Claude Opus 5.5. Carbon receives a score of 53.9% on the AutomationBench platform, which was developed by Zapier. The next closest competitor in that table is 42.5%.

In addition, Carbon leads Vending-Bench 2. The final amount that was simulated was $16,484 in the bank. GPT-6 Astra ended with $15,515, and Argon with $13,718. This is a target achievement. It does not display the revenue earned by a customer.

## How Much Gemini 4 Carbon Improved Over Argon

Carbon gains the most over Argon on exploit testing, long context, and science tasks. The table lists six of the largest jumps.

| **Benchmark** | **Argon** | **Carbon** | **Change** | **In plain terms** | 
|---|---|---|---|---|
| ExploitGym | 42.0% | 54.8% | +12.8 points | Better at proving a flaw can be exploited | 
| GraphWalks | 84.2% | 96.2% | +12.0 points | Better at tracking facts in huge inputs | 
| Terminal-Bench-Science 0.1 | 52.9% | 64.8% | +11.9 points | Better at scientific command-line work | 
| AA Intelligence Index v5.0.1 | 50 | 60 | +9 index points | Higher overall intelligence score | 
| Agents’ Last Exam v1.1 | 69.0% | 77.4% | +8.4 points | Better at multi-step agent tasks | 
| SWE-sweep | 4.3% | 9.6% | +5.3 points | More than double the software fix rate | 

Scores are Google-reported, published October 28, 2026. Changes are percentage points, except the AA Index, which uses index points. Sources: VentureBeat (October 28, 2026) and Layer3Labs (updated October 28, 2026).

The smallest wins reflect the constraints. Vals Index increased by just 1.2 points. Chartography went up 0.9 points. Carbon is an obvious improvement over some other materials and a minor one over others.

## Where GPT-6 Astra and Claude Opus 5.5 Lead

Two competitors took one win from Carbon each. GPT-6 Astra wins FrontierSWE 2, while Claude Opus 5.5 wins OSWorld-2.1.

On the other hand, GPT-6 Astra achieves a 65.5% accuracy on FrontierSWE 2. Claude Opus 5.5 scores 62.3%. Carbon scores 61.2%. This tests hard software engineering labor. Teams that predominantly ship code should test Astra and Opus 5.5 against Carbon for their own tasks.

On OSWorld-2.1, Claude Opus 5.5 scores 87.2%. Carbon scores 81.5%, and GPT-6 Astra scores 80.9%. The test measures a model's effectiveness at controlling a computer. Opus 5.5 is well in front in this area.

Otherwise, the margins are too narrow. On SWE-sweep, Carbon leads by one tenth of a point over Astra's 9.5%. On the Vals Index, Carbon’s 70.4% beats Opus 5.5’s 70.2% by two tenths. It's hard to get any ranges of uncertainty from Google's table, so call these low-win ties.

**Also Read:** Claude Opus 5.5 benchmarks, a score-by-score look at where it leads and where it trails.

## Gemini 4 Carbon Cybersecurity Benchmarks

During exploit testing, Carbon posts its largest security gains, but Google considered these scores without any guardrails. Google divided these into three categories: finding a flaw, checking it, and reliably fixing it.

| **Benchmark** | **Gemini 4 Carbon** | **Gemini 4 Argon** | **GPT-6 Astra** | **Claude Opus 5.5** | **What it tests** | 
|---|---|---|---|---|---|
| SEC-bench Pro | 88.2% | 83.4% | 85.4% | Not reported | Finding vulnerabilities | 
| ExploitGym | 54.8% | 42.0% | 42.4% | 34.5% | Showing a flaw can be exploited | 
| CWE-bench | 71.0% | 68.0% | 68.0% | 67.0% | Writing code fixes | 

Scores are Google-reported and were run without guardrails, per Google. Published October 28, 2026. Sources: Layer3Labs (updated October 28, 2026) and VentureBeat (October 28, 2026).

Carbon wins all three sets. The biggest difference is with ExploitGym, where it outscores the next best entry by more than 12 points.

It is important that “without guardrails” be noted. Guardrails are the constraints Google imposes on models used in production. The numbers given here are without any such constraints.

Google makes one more claim about its findings. According to Google, Carbon produces secure code at more than double the rate of Argon. Google also claims that Carbon produces unsupported claims half as often as Argon. These are Google’s own numbers. They are to be taken as claims to verify.

Google provides one more safety metric. In a Gray Swan prompt injection test, Carbon had an attack success rate of 0.0%, while Argon's was 0.7%.

## Independent Gemini 4 Carbon Benchmarks From Artificial Analysis

Carbon's superior general intelligence is now backed by independent testing, while indicators of factual reliability and coding agents point to weaker areas. In the Intelligence Index v5.0.1, Artificial Analysis ranks Carbon 4th. Claude Opus 5.5, GPT-6 Astra and Argon score 58, 57 and 50, respectively.

In version 5, the index was modified. Private exams or omitted responses now account for 60% of it, says Artificial Analysis. V5 scores should not be compared with older index scores.

The coding agents are another story. Carbon gets a rating of 60 on the Artificial Analysis Coding Agent Index in Google's Antigravity CLI. That is six points higher than Argon's 53. Claude Code (Opus 5.5) scores 62, leading.

The effects are mixed for knowledge work. On AA-Briefcase, Carbon improves to 1,609 Elo, despite a 126-point drop from Argon. Other models still perform better on that test, such as Kimi K3.1 Pro with a score of 1,678.

It is important to maintain factual reliability. On AA-Omniscience, Carbon is the highest-scoring player with a score of 54. It solves 58% of the questions correctly, compared to Argon's 48%. However, its hallucination rate increases from 4% to 11%. Artificial Analysis says that's wrong answers divided by all answers that aren't right. 11% is not 11% of the total number of answers.

## Gemini 4 Carbon Pricing and Access

Gemini 4 Carbon launched at the same introductory API rates as Gemini 4 Argon, but these rates will double on December 1, 2026. Developers can access Carbon through the paid Gemini API, while Google AI Ultra subscribers can use it through supported consumer products. Businesses can access it through Gemini for business.

| **Price per million tokens** | **Through November 30, 2026** | **From December 1, 2026** | 
|---|---|---|
| Input | $2 | $4 | 
| Output | $10 | $20 | 
| Cached input | About $0.10 (95% discount) | Not stated by Google | 

Pricing as of November 11, 2026. Prices are in USD. Google announced a 95% discount on cached input tokens. The introductory cached-input price of $0.10 is calculated from this discount. Google has not explicitly confirmed the cached-input rate after December 1.

### How the Pricing Changes Your API Costs

Consider a workload that processes 10 million input tokens and generates 1 million output tokens. At introductory rates, input costs $20, and output costs $10, for a total of $30.

From December 1, the same workload will cost $40 for input and $20 for output, for a total of $60. These estimates use standard API rates and exclude additional charges, such as applicable tools or services.

**Also Read:** Gemini 4 Carbon pricing for worked cost examples at both rate periods.

### Where Can You Access Gemini 4 Carbon?

- Developers: Through the paid Gemini API in Google AI Studio and Android Studio.
- Consumers: Through the Gemini app, AI Mode in Search and Google Antigravity for Google AI Ultra subscribers.
- Businesses: Through Gemini for business, including API and model-picker access, Google Antigravity and CodeMender.

### Does Gemini 4 Carbon Cost More to Run Than Argon?

Token pricing does not represent the total cost of completing a task. Google states that Carbon uses more tokens than Argon because it performs deeper reasoning and extended verification loops.

As a result, a complex coding or research task may consume more tokens even when both models have the same introductory rates. Google has not published a universal cost-per-completed-task figure for Carbon. Developers should measure token consumption, retries and successful task completion across their own workloads before estimating production costs.

## How to Use Gemini 4 Carbon Benchmarks to Pick a Model

Use benchmark scores to route tasks and not crown one winner. Match models to the use case they best suit.

- For long documents and large codebases: Test Carbon first. The best score in the table is Carbon's GraphWalks score.
- Security Review: Carbon is the top performer across the three published security rows. Retest under production guardrails.
- Using Desktop and computer-use agents: Test Claude Opus 5.5. It leads OSWorld-2.1.
- Try GPT-6 Astra and Claude Opus 5.5 on hard software engineering tests, and compare the results with Carbon.
- Outputs will be fact-heavy answers: watch closely. On AA-Omniscience, Carbon's hallucination rate increased.

Do a brief test on your own work. Use 20-50 authentic stimuli. Compare outcomes with the quality control. No one can predict how much work you will do.

*The majority of the rows are from Google's own table. Some agent tests and the Intelligence Index test have independent verification. Groups that require third-party evidence should wait for further independent runs.*

## Conclusion

The Gemini 4 Carbon benchmarks are clear about its limits and its strengths. It leads in general intelligence, long context, agents and security. It is tied for last place on one software engineering test and one computer-use test. The cybersecurity scores lack guardrails, and most of the scores are self-reported.

Scores are only useful if they become working software. For quick testing of ideas, use Emergent. Emergent is an AI App builder, You write your app in natural language, and it creates a full-stack web or mobile app. Combine benchmark research and a practical prototype. Start building on Emergent today.
