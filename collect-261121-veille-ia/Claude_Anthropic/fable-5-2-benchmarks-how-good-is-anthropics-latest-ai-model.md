---
id: collect-261121-veille-ia/claude-anthropic/fable-5-2-benchmarks-how-good-is-anthropics-latest-ai-model
title: "Fable 5.2 Benchmarks: How Good Is Anthropic’s Latest AI Model?"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "model", "anthropics", "claude", "agentic", "tokens", "coding", "research"]
source: https://emergent.sh/learn/fable-5-2-benchmarks
source_anchor: ""
source_lines: [1, 451]
section: "Claude_Anthropic"
---
#### TL;DR

- Coding: Fable 5.2 scores 49.1% on FrontierCode 2.0 and 69.9% on FrontierSWE v2.
- Computer use: It achieves 89.3% on OSWorld 2.1, outperforming Fable 5.1, Opus 5.5, and GPT-6 Astra in Anthropic’s comparison.
- Scientific research: It scores 74.6% on Terminal-Bench-Science 0.1.
- Agentic search: It reaches 82.1% on WANDR's relaxed score.
- Knowledge work: It records 66.1% on AA-Briefcase v1.1 using the rubric score.
- Pricing: API access starts at $8 per million input tokens and $40 per million output tokens.


Claude Fable 5.2 is Anthropic’s latest model for demanding reasoning, advanced software engineering, and long-running agentic workflows. Released on November 9, 2026, it brings improvements across coding, scientific research, computer use, and knowledge work.

But how much better is it than its predecessor, Fable 5.1? And how does it compare with Claude Opus 5.5 and other leading AI models?

Anthropic’s published benchmark results show Fable 5.2 scoring 49.1% on FrontierCode 2.0, 69.9% on FrontierSWE v2, and 89.3% on OSWorld 2.1. It also reports substantial gains in robotics, scientific research, and long-horizon agentic tasks.

This article examines the Fable 5.2 benchmarks, explains what each evaluation measures, and looks at what the results mean for developers and businesses.

The benchmark figures in this article are reported by Anthropic as of 10 November 2026. They should be treated as vendor-reported results rather than independently reproduced scores.

## What Is Claude Fable 5.2?

Claude Fable 5.2 is an advanced AI model from Anthropic designed for complex technical work, demanding reasoning, and long-horizon agentic tasks.

Unlike models intended primarily for everyday chat and routine coding, Fable 5.2 is built to handle projects that require extended planning, multiple steps, and sustained execution. These include large software migrations, multi-repository engineering tasks, scientific research, and complex analytical workflows.

Anthropic says the model can work for days with limited supervision. It can also coordinate hundreds of subagents, evaluate their results, and identify findings that do not reproduce.

Fable 5.2 and Claude Mythos 5.2 share the same underlying model. However, Mythos is available only to organizations approved through Anthropic's cybersecurity and life-sciences verification programs. Fable includes stronger safeguards for broader availability.

**Fable 5.2: Key specifications**

| **Specification** | **Details** | 
|---|---|
| Developer | Anthropic | 
| Model | Claude Fable 5.2 | 
| Release date | November 9, 2026 | 
| Model ID | claude-fable-5-2 | 
| Context window | 1 million tokens | 
| Maximum output | 128,000 tokens | 
| Input pricing | $8 per million tokens | 
| Output pricing | $40 per million tokens | 
| Thinking | Adaptive thinking | 
| Default effort | High | 
| Input modalities | Text and images | 
| Output modality | Text | 
| Best suited for | Complex reasoning, coding, research, and agentic workflows | 

Fable 5.2 uses adaptive thinking, which is always enabled. Developers can control reasoning depth through the effort setting. Its knowledge and training data cutoff is June 2026.

## Fable 5.2 Benchmarks: Overall Performance

Benchmarking of Anthropic’s models reveals better performance compared to Fable 5.1 for coding, agentic, computing usage, science, and knowledge work.

This comparison also involves Claude Opus 5.5, GPT-6 Astra, and Gemini 4 Carbon. Such a benchmark provides a more comprehensive perspective on the strengths and weaknesses of both Fable 5.2 and competing models.

**Fable 5.2 vs. Fable 5.1 and other AI models**

| **Benchmark** | **Fable 5.2** | **Fable 5.1** | **Opus 5.5** | **GPT-6 Astra** | **Gemini 4 Carbon** | 
|---|---|---|---|---|---|
| FrontierCode 2.0 | 49.1% | 32.4% | 45.5% | 42.8% | 41.1% | 
| FrontierSWE v2 | 69.9% | 56.3% | 62.3% | 65.5% | 61.2% | 
| AutomationBench | 51.4% | 31.4% | 42.5% | 41.4% | 53.9% | 
| Terminal-Bench-Science 0.1 | 74.6% | 52.6% | 58.7% | 68.1% | 64.8% | 
| OSWorld 2.1 | 89.3% | 83.5% | 87.2% | 79.5% | 81.5% | 
| RoboDojo Sim | 64.9% | 27.6% | 39.4% | 29.8% | Not reported | 
| AA-Briefcase v1.1 (rubric) | 66.1% | 61.5% | 61.2% | 52.0% | 61.8% | 
| Remote Labor Index | 41.3% | 17.9% | 21.3% | 20.8% | Not reported | 

*Source: Anthropic's November 9, 2026 model announcement. Scores are from the highest available effort for each model unless otherwise noted. Fable 5.2, Fable 5.1, and Opus 5.5 results use safeguards enabled. Some evaluations use specific effort settings or partial test sets.*

Fable 5.2 leads in most of the listed comparisons, but it does not win every evaluation. Gemini 4 Carbon scores higher on AutomationBench, with 53.9% compared with Fable 5.2's 51.4%.

The results also show a substantial improvement over Fable 5.1. For example, Fable 5.2 improves from 32.4% to 49.1% on FrontierCode 2.0 and from 83.5% to 89.3% on OSWorld 2.1.

These results suggest that the upgrade is not limited to coding. The model also shows gains in computer use, robotics, research, and knowledge-intensive tasks.

## Fable 5.2 Coding Benchmarks

Coding is one of the most important areas in Fable 5.2's evaluation. The model is designed to handle complex software engineering work, including repository-level changes, debugging, code migrations, and tasks that require multiple rounds of testing.

Anthropic reports results across two major software engineering benchmarks: FrontierCode 2.0 and FrontierSWE v2.

### **1. FrontierCode 2.0**

49.1% — Fable 5.2

FrontierCode 2.0 evaluates agentic coding performance by measuring whether a model's proposed code changes would be accepted by maintainers of open-source repositories.

Benchmarking will consider practical software engineering issues and not just the question of code generation alone. The models should be able to analyze the existing software, modify it appropriately, and give solutions that are repository compliant.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 49.1% | 
| Opus 5.5 | 45.5% | 
| GPT-6 Astra | 42.8% | 
| Gemini 4 Carbon | 41.1% | 
| Fable 5.1 | 32.4% | 

Fable 5.2 leads this comparison, with a 16.7-percentage-point improvement over Fable 5.1.

Anthropic also reports that Fable 5.2 reaches 49.1% at Xhigh effort, the highest score among the released models it tested. At High effort, it scores 45.3%, approximately matching Opus 5.5's 45.5% result while using roughly half the cost per task in Anthropic's early testing.

anthropic.com

### 2. FrontierSWE v2

69.9% — Fable 5.2

FrontierSWE v2 evaluates an AI model's ability to complete software engineering tasks in real-world repository environments.

Unlike simple programming tests, repository-level tasks require models to understand existing code, locate relevant files, identify the underlying issue, and implement changes without disrupting unrelated functionality.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 69.9% | 
| GPT-6 Astra | 65.5% | 
| Opus 5.5 | 62.3% | 
| Gemini 4 Carbon | 61.2% | 
| Fable 5.1 | 56.3% | 

Fable 5.2 improves by 13.6 percentage points over its predecessor. It also leads GPT-6 Astra by 4.4 percentage points in Anthropic's comparison.

This result is relevant to developers building coding agents that need to work across larger repositories rather than generate standalone functions.

anthropic.com

**What do the coding results mean?**

The coding benchmarks indicate that Fable 5.2 is better equipped for complex software engineering tasks than Fable 5.1.

Its performance is particularly relevant to:

- Repository-level development: Understanding and modifying existing codebases.
- Large code migrations: Updating multiple components while preserving existing functionality.
- Debugging: Identifying underlying issues instead of applying superficial fixes.
- Automated testing: Iterating on code and checking whether changes work as intended.
- Coding agents: Completing multi-step development tasks with less continuous supervision.

However, benchmark performance does not guarantee that every generated patch will be correct or production-ready. Developers should still review code, run tests, and validate changes before deployment.

To build that review step into the workflow, a coding agent builder lets you set the steps and where a person signs off before a change ships.

## Fable 5.2 Agentic Benchmarks

Agentic AI systems can plan tasks, use tools, interact with software, and complete multi-step workflows. These capabilities are important when an AI model needs to do more than respond to a single prompt.

Fable 5.2 is designed for long-running agentic work, making its performance on automation, search, and computer-use evaluations particularly relevant.

### 1. AutomationBench

51.4% — Fable 5.2

AutomationBench evaluates how well AI models handle business workflows and automation tasks.

These workflows can involve multiple steps, decisions, and interactions with tools. The benchmark is useful for assessing models that need to complete structured business processes rather than simply explain how to perform them.

| **Model** | **Score** | 
|---|---|
| Gemini 4 Carbon | 53.9% | 
| Fable 5.2 | 51.4% | 
| Opus 5.5 | 42.5% | 
| GPT-6 Astra | 41.4% | 
| Fable 5.1 | 31.4% | 

Fable 5.2 improves by 20 percentage points over Fable 5.1. However, Gemini 4 Carbon takes the lead with a score of 53.9%.

This result shows that Fable 5.2 has made significant progress in business automation, although it is not the top-performing model in this particular evaluation.

### 2. WANDR: Agentic search

82.1% — Fable 5.2

WANDR evaluates AI agents on large-scale data collection tasks. It measures how effectively a model can gather information across multiple steps and work toward a broader research objective.

Anthropic reports the following relaxed scores:

| **Model** | **WANDR score** | 
|---|---|
| Fable 5.2 | 82.1% | 
| Opus 5.5 | 72.3% | 
| Fable 5.1 | 68.7% | 

Fable 5.2 leads Opus 5.5 by 9.8 percentage points and improves by 13.4 percentage points over Fable 5.1.

Anthropic also reports that Fable 5.2 at Medium effort matches Opus 5.5's top score at approximately 40% of the cost per task. This suggests that the model can deliver strong agentic search performance without always requiring its highest effort setting.

### **3. OSWorld 2.1: Computer use**

89.3% — Fable 5.2

OSWorld 2.1 measures an AI model's ability to interact with computer interfaces and complete tasks in desktop environments.

Computer-use capabilities are important for agents that need to navigate applications, operate software, and perform actions through graphical interfaces.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 89.3% | 
| Opus 5.5 | 87.2% | 
| Fable 5.1 | 83.5% | 
| Gemini 4 Carbon | 81.5% | 
| GPT-6 Astra | 79.5% | 

Fable 5.2 improves by 5.8 percentage points over Fable 5.1. It also leads Opus 5.5 by 2.1 percentage points.

This makes OSWorld 2.1 one of the strongest results in the benchmark set for evaluating Fable 5.2's ability to interact with software. The result is based on an offline, partial evaluation set, so it should not be interpreted as a universal computer-use success rate.

## Fable 5.2 Science and Reasoning Benchmarks

Fable 5.2 is not limited to software engineering and automation. Anthropic also reports improvements in scientific research, multidisciplinary reasoning, and knowledge-intensive tasks.

These evaluations are useful for understanding whether the model can work with complex information, reason through difficult problems, and support research workflows.

### 1. Terminal-Bench-Science 0.1

74.6% — Fable 5.2

Terminal-Bench-Science 0.1 evaluates an AI agent's ability to complete scientific research tasks in a terminal-based environment.

It tests whether models can work through technical research workflows involving tools and multi-step problem-solving.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 74.6% | 
| GPT-6 Astra | 68.1% | 
| Gemini 4 Carbon | 64.8% | 
| Opus 5.5 | 58.7% | 
| Fable 5.1 | 52.6% | 

Fable 5.2 leads the comparison, improving by 22 percentage points over Fable 5.1. It also outperforms GPT-6 Astra by 6.5 percentage points.

This result supports Anthropic's positioning of Fable 5.2 as a model for technical research and scientific workflows.

### 2. Humanity's Last Exam

74.2% — Fable 5.2

Humanity's Last Exam (HLE) evaluates advanced academic knowledge and multidisciplinary reasoning. The version reported here uses tools, allowing models to work through challenging questions with external assistance.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 74.2% | 
| Opus 5.5 | 67.7% | 
| Fable 5.1 | 65.6% | 
| GPT-6 Astra | 57.2% | 
| Gemini 4 Carbon | Not reported | 

Fable 5.2 improves by 8.6 percentage points over Fable 5.1 and leads Opus 5.5 by 6.5 percentage points.

Anthropic also reports that at Low effort, Fable 5.2 matches Opus 5.5's top score for less than one-tenth of the cost per task. This highlights the importance of reasoning-effort settings when evaluating the model's overall efficiency.

anthropic.com

### 3. AA-Briefcase v1.1: Knowledge work

66.1% — Fable 5.2

AA-Briefcase v1.1 evaluates knowledge-work performance using a rubric-based scoring method. It assesses how effectively a model handles tasks that require research, analysis, and structured output.

| **Model** | **Rubric score** | **Elo** | 
|---|---|---|
| Fable 5.2 | 66.1% | 1,785 | 
| Gemini 4 Carbon | 61.8% | 1,611 | 
| Fable 5.1 | 61.5% | 1,669 | 
| Opus 5.5 | 61.2% | 1,800 | 
| GPT-6 Astra | 52.0% | 1,569 | 

Fable 5.2 achieves the highest rubric score in this comparison. However, Opus 5.5 has a higher Elo rating of 1,800 compared with Fable 5.2's 1,785.

These are two different scoring measures, so they should not be treated as interchangeable. The rubric score reflects performance under the benchmark's evaluation criteria, while Elo represents a relative rating.

### 4. Remote Labor Index

41.3% — Fable 5.2

The Remote Labor Index (RLI) evaluates how well an AI model performs work that resembles tasks completed by human workers.

Anthropic reports the following results:

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 41.3% | 
| Opus 5.5 | 21.3% | 
| GPT-6 Astra | 20.8% | 
| Fable 5.1 | 17.9% | 
| Gemini 4 Carbon | Not reported | 

Fable 5.2's score is 23.4 percentage points higher than Fable 5.1's. It also nearly doubles Opus 5.5's reported score.

The result is particularly relevant to businesses exploring AI systems for research, analysis, and knowledge-work automation.

## Fable 5.2 Robotics and Visual Understanding Benchmarks

Anthropic also reports improvements in robotics and visual understanding. These capabilities extend the model's potential beyond conventional text-based applications.

**1. RoboDojo Sim**

64.9% — Fable 5.2

RoboDojo Sim evaluates model performance in simulated robotics environments. It provides insight into how effectively a model can work through tasks involving robotic systems and their environments.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 64.9% | 
| Opus 5.5 | 39.4% | 
| GPT-6 Astra | 29.8% | 
| Fable 5.1 | 27.6% | 
| Gemini 4 Carbon | Not reported | 

Fable 5.2 improves by 37.3 percentage points over Fable 5.1. This is one of the largest improvements in Anthropic's published comparison.

Anthropic also describes Fable 5.2's progress in robotics evaluations during prerelease testing. However, the RoboDojo Sim result is an internal evaluation and should not be interpreted as evidence of equivalent performance on physical robots.

anthropic.com

**2. Chartography**

73.4% — Fable 5.2

Chartography evaluates how accurately a model interprets charts and figures without using external tools.

This capability is useful for tasks involving dashboards, financial reports, scientific figures, and visual data analysis.

| **Model** | **Score** | 
|---|---|
| Fable 5.2 | 73.4% | 
| Gemini 4 Carbon | 72.5% | 
| GPT-6 Astra | 71.0% | 
| Opus 5.5 | 64.4% | 
| Fable 5.1 | 44.8% | 

Fable 5.2 leads the comparison by 0.9 percentage points over Gemini 4 Carbon. It also improves by 28.6 percentage points over Fable 5.1.

Anthropic reports an additional efficiency result: at Medium effort, Fable 5.2 scores 66.4%, exceeding Opus 5.5 at Max effort, which scores 64.4%, for less than one-tenth of the cost per task. These are separate effort configurations from the highest-effort comparison above.

anthropic.com

## **Fable 5.2 vs. Fable 5.1: What Has Improved?**

The benchmark results show that Fable 5.2 is a substantial upgrade over Fable 5.1. Its improvements extend across software engineering, agentic workflows, research, computer use, and robotics.

Here is a summary of the percentage-point improvements across selected evaluations.

| **Benchmark** | **Fable 5.1** | **Fable 5.2** | **Improvement** | 
|---|---|---|---|
| FrontierCode 2.0 | 32.4% | 49.1% | +16.7 points | 
| FrontierSWE v2 | 56.3% | 69.9% | +13.6 points | 
| AutomationBench | 31.4% | 51.4% | +20.0 points | 
| Terminal-Bench-Science 0.1 | 52.6% | 74.6% | +22.0 points | 
| OSWorld 2.1 | 83.5% | 89.3% | +5.8 points | 
| RoboDojo Sim | 27.6% | 64.9% | +37.3 points | 
| AA-Briefcase v1.1 | 61.5% | 66.1% | +4.6 points | 
| Remote Labor Index | 17.9% | 41.3% | +23.4 points | 

Improvements are calculated by subtracting the Fable 5.1 score from the Fable 5.2 score. They represent percentage-point differences, not relative percentage growth.

anthropic.com

Three improvements stand out:

- Robotics: The 37.3-point increase on RoboDojo Sim is the largest improvement in this selection.
- Knowledge work and research: The Remote Labor Index improves by 23.4 points, while Terminal-Bench-Science increases by 22 points.
- Coding: FrontierCode 2.0 improves by 16.7 points, showing a substantial gain in agentic software engineering.

The improvements suggest that Fable 5.2 is not simply a faster version of its predecessor. It delivers stronger results across several different categories of work.

For the scores Anthropic published for the earlier model, see our Claude Fable 5.1 benchmarks guide.

## Fable 5.2 Speed, Efficiency, and Pricing

Accurate benchmarking is just one facet of performance. Response time, reasoning costs, token cost, and API costs are other factors that can play into a model's usability by enterprises and developers.

**How much faster is Fable 5.2?**

Anthropic reports that Fable 5.2 generates outputs up to 30% faster than Fable 5.1.

The model also uses fewer tokens to complete challenging tasks at Medium effort compared with its predecessor. This can help reduce the cost of complex workflows, particularly when the model needs to reason through multiple steps.

Fable 5.2 also offers an optional Fast mode, which can increase token generation speed by up to 2.5 times. However, Fast mode comes at a premium price.

**Fable 5.2 API pricing**

| **Pricing component** | **Standard mode** | **Fast mode** | 
|---|---|---|
| Input tokens | $8 / MTok | $16 / MTok | 
| Output tokens | $40 / MTok | $80 / MTok | 
| Cache read | $0.20 / MTok | Confirm applicable pricing | 
| 5-minute cache write | $10 / MTok | Confirm applicable pricing | 
| 1-hour cache write | $16 / MTok | Confirm applicable pricing | 

MTok means one million tokens. Standard input and output rates are from Anthropic's published pricing. Fast mode doubles the standard input and output rates. Check the current pricing documentation for caching and other billing details before deployment.

**Also Read:** Claude Opus 5.5 pricing for how its token rates and caching compare.

**How do effort settings affect performance?**

Fable 5.2 uses adaptive thinking, which is always enabled. Developers can control the depth of reasoning through effort settings.

The available effort levels include:

- Low: Suitable when a task needs less reasoning depth and faster completion.
- Medium: A practical starting point for many workflows.
- High: Useful for demanding tasks that need more careful reasoning.
- Xhigh: Designed for particularly challenging tasks where higher performance is worth additional time and cost.
- Max: Available in specific evaluation and configuration contexts.

Higher effort does not automatically mean better value. For example, Anthropic reports that Fable 5.2 at High effort achieves 45.3% on FrontierCode 2.0, close to Opus 5.5's 45.5% result at its best configuration, for roughly half the cost per task in early testing.

This makes it important to evaluate performance at the effort level your application will actually use rather than relying only on maximum-effort benchmark results.

**Also Read:** Claude Fable 5.1 high vs low effort for how effort changes cost and results on the earlier model.

## Where Can You Access Fable 5.2?

Fable 5.2 is available through Anthropic's API and supported cloud platforms.

The model uses the API identifier claude-fable-5-2. It has a 1-million-token context window and supports a maximum output of 128,000 tokens.

Fable 5.2 also has specific data-retention requirements. Anthropic's platform documentation states that the model requires 30-day data retention and eligible customers can use ZDR before implementing Enterprise Frontier Safeguards, and ZDR remains compatible with EFS. Organizations should review the applicable platform and contractual policies before sending sensitive information.

## Who Should Use Fable 5.2?

Fable 5.2 is designed for demanding technical and knowledge-intensive work. Its benchmark results make it worth considering for teams that need more than routine text generation.

| **Use case** | **Why Fable 5.2 fits** | 
|---|---|
| Complex software engineering | Strong FrontierCode 2.0 and FrontierSWE v2 results | 
| Large code migrations | Designed for long-running, multi-repository engineering tasks | 
| Coding agents | Strong performance on agentic coding evaluations | 
| Business automation | Improved AutomationBench results over Fable 5.1 | 
| Computer-use agents | 89.3% on OSWorld 2.1 | 
| Scientific research | 74.6% on Terminal-Bench-Science 0.1 | 
| Data collection and research | 82.1% on WANDR | 
| Knowledge work | 66.1% on AA-Briefcase v1.1 | 
| Visual data analysis | 73.4% on Chartography | 

However, Fable 5.2 may be more capable than necessary for everyday tasks. Anthropic recommends starting with Opus 5.5 for most coding and knowledge-work requirements, then moving to Fable 5.2 when higher effort on Opus does not meet the task's requirements.

**Also Read:** Claude Fable 5.1 vs Opus 5 for how the tier above Opus compares on price and results

## Final Verdict: Is Fable 5.2 Worth Using?

Fable 5.2 delivers substantial improvements over Fable 5.1 across coding, reasoning, agentic workflows, computer use, and scientific research.

Its 49.1% FrontierCode 2.0 score and 69.9% FrontierSWE v2 result make it a strong option for complex software engineering. The 89.3% OSWorld 2.1 score also highlights its ability to interact with computer environments, while its performance on Terminal-Bench-Science and WANDR supports its positioning for research-heavy tasks.

Nevertheless, because of its cost of $8 per million input tokens and $40 per million output tokens, it is an expensive model. Teams need to consider whether its enhanced capabilities are worth the extra cost relative to Opus 5.5 and smaller models.

For developers and businesses, the practical approach is to test Fable 5.2 on real workflows, compare results at different effort levels, and measure total task cost before adopting it at scale.

Fable 5.2 is now live on Emergent! You can now access Fable 5.2 directly on Emergent and use its AI capabilities to build applications, power intelligent workflows, and turn your ideas into functional products. Start building with Fable 5.2 today!
