---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-benchmarks-pricing-and-claude-comparison
title: "Kimi K3.1 Pro: benchmarks, pricing and Claude comparison"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["kimi", "claude", "test", "index", "models", "table", "tests", "marathon"]
source: https://heymark.ai/en/guides/kimi-k3-1-pro-benchmarks-pricing/
source_anchor: ""
source_lines: [1, 161]
section: "Kimi"
---
**Kimi K3.1 Pro scored 51.7% on SWE Marathon, ahead of Claude Opus 5.5 at 46.3%**, in the table Moonshot published for its October 23 launch. These are provider-compiled results: the table evaluates Kimi with Claude Code and collects published rival scores. The model is now available through Kimi’s products and API.

If you’re considering alternatives to Claude or GPT for coding, that result deserves a look. The useful question is how much of that advantage carries over to your work and what you pay for it. We compare published tests, two official demos and API costs.

## Where does it beat Claude, and where does it trail?

A separate evaluation by Artificial Analysis puts Kimi at **67% on Terminal-Bench 4.0, against Claude Sonnet 5.5 at 64%** and Kimi K3 at 13%. This test asks agents to complete tasks in a terminal. It is an independent result distinct from the headline’s SWE Marathon comparison.

The wider ranking tells a different story. Kimi reaches **55 points and fourth place on the Intelligence Index**. Claude Opus 5.5 scores **58**; Sonnet 5.5 and Muse Wonder Full score **56**. The report identifies reasoning settings and fallback behavior for the Claude models. Results and conditions.

The official table covers 14 tests. This summary calculates each model’s average rank and expresses it as a 0–100 index. Switch between four models across all 14 tests or six models across their nine shared tests. Each view uses the same selection for every participant.

Calculated from Moonshot’s table. Higher means better positions within the cohort.

Fable and GLM have missing results; include them in the nine-shared-tests view.

**78.6**

**69**

**45.2**

**—**

**7.1**

**—**

The index summarizes ranks; it does not measure the size of a lead or accuracy. The views use different cohorts and are not comparable.

## View all 14 benchmarks and original scores

Scrollable table · — no published result

| Test · unit | Kimi K3.1 Pro | Opus 5.5 | GPT-6 Astra | Fable 5.1 | DeepSeek V4.1 Flash 1017 | GLM-5.3 | 
|---|---|---|---|---|---|---|
| Terminal-Bench 4.0% | 65.7 | **66.4** | 58.2 | 57.9 | 42.3 | 41.8 | 
| SWE Marathon% | **51.7** | 46.3 | 42.5 | 45.6 | 40 | 42.5 | 
| Terminal-Bench-Science 0.1% | 38.1 | 63.3 | **68.1** | 40 | 15.7 | 8.1 | 
| Kimi Code Bench 3.0 Long-horizon% | 61.2 | 58.5 | **66.5** | 58.8 | 51.5 | 45.5 | 
| Agents’ Last Exam% | **43.2** | 42.6 | 41.8 | — | 37.7 | 26.2 | 
| BenchCAD w/ tools% | 92.7 | **96.2** | 95.9 | 92.6 | 81.6 | — | 
| OfficeQA Pro v2% | **64.4** | 62.2 | 54.4 | 54.5 | 40.4 | 31.1 | 
| Frontier Finance% | 58.6 | 56.7 | 45 | **60.8** | 54.5 | 57.5 | 
| GDPval-AA v2.1Elo | 1,846 | **1,866** | 1,573 | 1,754 | 1,746 | 1,650 | 
| OSWorld V2.1% | 79.6 | **81.8** | 75.8 | 80.7 | 69.3 | — | 
| Humanity’s Last Exam Full w/ tools% | **70** | 67.7 | 57.2 | 65.6 | 58.8 | 62.5 | 
| GDP.pdf% | **34.6** | 30.6 | 34.2 | 27.6 | 17.2 | 11.2 | 
| ChartoGraphy w/ tools% | 90.2 | 89 | **91** | 88.4 | 83 | — | 
| ZeroBench_main w/ tools (pass@5)% | **63** | 61 | 58 | 59 | 51 | — | 

## How we calculate the summary and what it includes

Editorial position index calculated by HeyMark: in each test, models are ranked from highest to lowest score. Ties share the average rank. We average positions with equal weight per test and map them to 0–100: 100 would mean first in every test; 0, last in every test. It does not measure the size of the lead and is neither accuracy nor an independent benchmark. Changing the cohort changes the index; the two views are not comparable.

Formula: index = 100 × (N − average rank) / (N − 1), where N is the number of models in the cohort.

The 14 tests include results compiled by Moonshot from different harnesses, reports and evaluators. The six-model view includes Terminal-Bench, SWE Marathon, Terminal-Bench-Science, Kimi Code Bench, OfficeQA, Frontier Finance, GDPval-AA, HLE and GDP.pdf. Artificial Analysis’s independent 67% result is excluded to avoid counting Terminal-Bench twice.

**Terminal-Bench 4.0:** Kimi: Claude Code, max effort. Rivals: highest figure from release posts or the leaderboard.

**SWE Marathon:** Kimi and DeepSeek use Claude Code; other results come from the SWE-Marathon leaderboard.

**Terminal-Bench-Science 0.1:** Kimi uses Claude Code. Moonshot lists DeepSeek’s 15.7% leaderboard result, obtained with an earlier Flash version.

**Kimi Code Bench 3.0 Long-horizon:** Internal Moonshot evaluation. Maximum effort; Claude runs may include fallback.

**Agents’ Last Exam:** 122 tasks. DeepSeek uses Claude Code; Kimi uses Kimi Code. Other figures come from the benchmark site.

**BenchCAD w/ tools:** Voxel IoU: geometric overlap. Kimi is evaluated on a 1,000-file subset; rival scores come from their reports.

**OfficeQA Pro v2:** Astra uses Codex; others use Claude Code. PDFs are rendered as images, with checks for online answer retrieval.

**Frontier Finance:** Official finance agent v2 harness.

**GDPval-AA v2.1:** Artificial Analysis results cited in Moonshot’s table. Elo scale.

**OSWorld V2.1:** Kimi uses its Agent harness; Astra’s figure is taken from the Muse Wonder technical report.

**Humanity’s Last Exam Full w/ tools:** HLE Full with tools, from Moonshot’s figure. Different from AA’s independent 52% evaluation.

**GDP.pdf:** With Python, averaged over five runs; Moonshot’s table protocol.

**ChartoGraphy w/ tools:** With Python, averaged over five runs; Moonshot’s table protocol.

**ZeroBench_main w/ tools (pass@5):** With Python, averaged over five runs; pass@5 metric.

HeyMark calculation from Moonshot’s results published on 23.10.2026. Maximum effort; Claude may include fallback and GPT may include cyber safeguards.

**Why does Kimi appear at both 67% and 65.7%?** These come from different evaluations. Artificial Analysis reports the first; Moonshot’s table reports the second, evaluating Kimi with Claude Code and collecting published rival results. Combining them into one ranking would erase those conditions. Moonshot’s methodology.

When deciding whether to switch, start with a task you already know, check the output and record the full cost, including corrections. A lower token price can still produce a larger bill if the agent needs more attempts.

## Two official demos you can inspect

Moonshot published examples of work with 3D tools. The videos below are demonstrations selected by the company. Their production involves software, libraries and external assets; you can inspect the results without attributing every visual element to the model.

### An airport built in Blender

The first demo shows an airport’s indoor rainforest. Moonshot lists Blender, Python, Geometry Nodes and Cycles among the tools. It credits the Poly Haven sky HDRI to Greg Zaal and Jarod Guest under CC0.

### An engine with an exploded view

The second uses FreeCAD and Python. Its exploded view lets you inspect how the engine geometry is organized. This is a modeling example; the animation alone does not establish that the design would function as a real engine.

Both examples, their descriptions and additional projects are available in the original technical announcement.

## What is 3D Make?

Kimi also introduces **3D Make** on its website and app: you can preview and edit models, download them for 3D printing, or export videos and Live Photos. The image below shows that tool. Announced features.

For a creator, distinguish an editable 3D file from a video that only shows its appearance. Before using an asset in a project, inspect geometry, materials, source rights and the formats your editor accepts. An attractive preview cannot establish all of that.

## Kimi K3.1 Pro and Highspeed pricing

The table shows **API rates** checked on October 26. Cache reads have their own rate; writes depend on how long the cache is retained. Kimi membership is billed separately. Billing documentation.

| USD per 1 million tokens · before tax |  |  |  |  |  | 
|---|---|---|---|---|---|
| Model | Input | Output | Cache read | 5-min write | 1-hour write | 
|---|---|---|---|---|---|
| K3.1 Pro | $2 | $10 | $0.20 | $2 | $4 | 
| K3.1 Pro Highspeed | $4 | $20 | $0.20 | $4 | $8 | 

Official rates ↗ · 26.10.2026

### What does a request cost?

What you send uses input tokens; the response uses output tokens. Adjust the amounts to compare both versions.

Enter whole amounts from 0, up to a combined 1,048,576 tokens.

- K3.1 Pro
- USD 0.04
- K3.1 Pro Highspeed
- USD 0.08

Calculated example, without caching or tools. Token amounts are declared quantities, not a measured sample text. Cache writes and other services may add charges.

Moonshot describes Highspeed as **2–3 times faster**. That is the provider’s claim; completion time also depends on tools and how much work the agent actually performs. Announcement.

## Where to use it, and when the weights arrive

| Channel | What you can use | Condition | 
|---|---|---|
| **Kimi web and app** | K3.1 Pro and 3D Make, according to the launch. | Check your account limits and each plan’s benefits. | 
| **Kimi Work and Kimi Code** | File workflows and coding. | Work requires version 3.1.0 or later; Code lets you select the model. | 
| **API** | `kimi-k3.1-pro` and`kimi-k3.1-pro-highspeed` . | The guide requires a successful top-up of at least US$1 to unlock this model. | 
| **Model weights** | Release promised by **November 4, 2026** . | Publication remained pending in the sources reviewed. | 

Sources: launch availability, API requirements and Kimi plans. The plans page includes Highspeed in Pro, Max and Ultra; check monthly versus annual billing before comparing prices.

The API supports **low, high and max** reasoning effort, defaulting to max. The sources we used do not publish a complete curve for each setting, so these results cannot tell you what happens when you switch to low. Model guide.

## The original announcement

## Source

## Plan your next post in HeyMark.

Keep the idea, review the draft with your team, and see how it performed in the accounts you connected.
