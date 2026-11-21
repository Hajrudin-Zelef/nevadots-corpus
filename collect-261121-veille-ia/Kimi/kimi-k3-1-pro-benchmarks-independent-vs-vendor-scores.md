---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-benchmarks-independent-vs-vendor-scores
title: "Kimi K3.1 Pro Benchmarks, Independent vs Vendor Scores"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["kimi", "moonshot", "artificial", "analysis", "index", "independent", "tokens", "scores"]
source: https://www.layer3labs.io/guides/kimi-k3-1-pro-benchmarks
source_anchor: ""
source_lines: [1, 77]
section: "Kimi"
---
# Kimi K3.1 Pro Benchmarks: Independent Scores and Vendor Claims

Artificial Analysis results and Moonshot's own test numbers, kept apart, plus a way to test it yourself.

The independent Artificial Analysis index gives Kimi K3.1 Pro an Intelligence Index score of 55, ranking it fourth out of 215 evaluated models. At Layer3Labs, we build custom agents and enterprise integrations, so cost per finished task matters more to us than any leaderboard rank.

Vendor-reported numbers from Moonshot AI place Kimi K3.1 Pro within 0.7 percentage points of Claude Opus 5.5 on Terminal-Bench 4.0, reaching 65.7 percent compared to 66.4 percent. Some community posters say real-world use trails the index.

## Kimi K3.1 Pro Benchmark Scores

Kimi K3.1 Pro benchmark scores diverge depending on whether you inspect independent third-party evaluations or evaluation runs published directly by Moonshot AI. Third-party testing by Artificial Analysis ranks Kimi K3.1 Pro fourth of 215 models, while Moonshot internal evaluations report competitive gains over earlier models.

On the independent Artificial Analysis Intelligence Index, Kimi K3.1 Pro scores 55 and ranks fourth of 215 tested models. One community headline puts it 3 points behind Claude Opus 5.5. On vendor-managed harnesses, Moonshot reports 65.7 percent on Terminal-Bench 4.0, which sits ahead of GPT-6 Astra at 58.2 percent and just behind Claude Opus 5.5 at 66.4 percent.

- Artificial Analysis Intelligence Index: 55 (Rank 4 of 215, run independently by Artificial Analysis)
- Terminal-Bench 4.0: 65.7% for K3.1 Pro vs 66.4% for Claude Opus 5.5 and 58.2% for GPT-6 Astra (run by Moonshot)
- BenchCAD with tools: 92.7% for K3.1 Pro vs 96.2% for Claude Opus 5.5 and 95.9% for GPT-6 Astra (run by Moonshot)
- OfficeQA Pro v2: 64.4 score for K3.1 Pro, reported as state of the art (run by Moonshot)
- Kimi Code Bench 3.0 Long-horizon: 61.2% for K3.1 Pro vs 55.95% for predecessor Kimi K3 (internal Moonshot benchmark)
- Kimi Hallu Bench Hard: 8.3% claim-level hallucination rate for K3.1 Pro vs 23.4% for K3 (internal stress test by Moonshot)

## What the Independent Index Shows

Output speed is 57.5 tokens per second, ranked 141 of 186 models. Artificial Analysis calls that notably slow. By comparison, Moonshot offers Kimi K3.1 Pro Highspeed on the Kimi API, claiming two to three times the generation speed of standard K3.1 Pro.

The model is also very verbose. Artificial Analysis recorded 150 million output tokens used to evaluate Kimi K3.1 Pro across the index, compared to a median of 87 million tokens. Artificial Analysis puts the cost of running the index at about $2.11 per task. It lists the model at $2.00 per million input tokens and $10.00 per million output tokens.

Artificial Analysis lists Kimi K3.1 Pro as a proprietary model on its tracker, while Moonshot calls it open-weight with weights pending. Moonshot says full weights are to be released by November 4, 2026. They are not out yet.

- Intelligence Index score: 55, ranking fourth among 215 models
- Generation speed: 57.5 tokens per second, ranking 141 of 186 models
- Token consumption: 150 million output tokens to complete the index suite
- Evaluation cost: approximately $2.11 per benchmark task
- Context window: 1M tokens, listed by Artificial Analysis

## What Moonshot Reports Across Test Suites

Moonshot says K3.1 Pro is K3 with substantially larger post-training compute and joint reinforcement learning across hundreds of downstream domains.

On domain-specific evaluations, Moonshot reports state of the art performance on OfficeQA Pro v2 with a score of 64.4. Internal finance benchmarks show gains over Kimi K3, including an increase of 7.2 points on Financial Modeling, an increase of 11.6 points on Financial Research, and about 4.2x on Business Operation.

On spatial and engineering workflows, Moonshot evaluated Kimi K3.1 Pro on BenchCAD using tool integrations, scoring 92.7 percent compared to 96.2 percent for Claude Opus 5.5. Moonshot says the model works across Unity, Unreal Engine and Godot. It also says the model can write and run Blender and CAD scripts without MCP connectors.

Moonshot also published results on Kimi Hallu Bench Hard, an internal stress test that Moonshot says is not an overall error rate. Under this stress harness, Kimi K3.1 Pro demonstrated a claim-level hallucination rate of 8.3 percent, reduced from 23.4 percent on Kimi K3.

On xdev-bench-hard, Moonshot's in-house vulnerability benchmark (pass@3), a checkpoint without the cyber-data removal solves 90.5% and the final model reaches 81.0%. Moonshot says its offensive cyber capability remains behind frontier proprietary models that have been public for at least three months.

## The Real-World Performance Dispute

A ModelCompare headline says Kimi K3.1 Pro puts open-source AI a month behind the best. Community forum posts add that it does not look much better than DeepSeek 4.1 Flash, seems slightly worse than GPT 6.1 Sol at certain tasks, and that the index does not represent real-world performance. These are opinions, not measurements.

## How to Test Kimi K3.1 Pro for Your Work

Testing Kimi K3.1 Pro against your actual workload requires measuring cost per completed business task rather than comparing nominal per-token rates. Artificial Analysis recorded 150 million output tokens against an 87 million median, so budget for high output volume.

Begin by running 10 to 20 representative enterprise tasks through the Kimi API using the kimi-k3.1-pro model identifier. You can configure the reasoning_effort parameter to low, high, or max to observe how output token length and task accuracy adjust on your prompts.

Track total completion cost alongside accuracy. At $2.00 per million input tokens, $10.00 per million output tokens, and $0.20 per million cached input tokens, Kimi K3.1 Pro is priced lower than predecessor Kimi K3, which billed at $3.00 input and $15.00 output. Review our detailed breakdown in the Kimi K3.1 Pro pricing guide to calculate projected batch costs.

If you are assessing code automation workflows, compare results directly with our Kimi K3 benchmarks guide and our architectural overview in the Kimi K3.1 Pro explained guide. Organizations with compliance constraints should complete a data residency review with counsel before sending proprietary files to the API. Zero Data Retention is available for API customers.

- Select 10 to 20 actual production prompts representing complex documents or coding tasks
- Set reasoning_effort across test batches to compare response time against quality
- Calculate total expenditure based on finished task volume rather than raw input token counts
- Verify context caching efficiency using the five-minute default Time to Live (TTL) on repeated prefixes

## Frequently Asked Questions

- Not on the scores Moonshot publishes for Terminal-Bench 4.0 (65.7% vs 66.4%) or BenchCAD with tools (92.7% vs 96.2%). On the independent Artificial Analysis Intelligence Index, Kimi K3.1 Pro scores 55, and a community headline puts it 3 points behind Claude Opus 5.5 on the Artificial Analysis index.
- Artificial Analysis is a third-party index and ranks Kimi K3.1 Pro 4th of 215 at 55, while also rating it slow and very verbose. Some community posters argue the index does not represent real-world performance, so treat it as one input and run your own tasks.
- Moonshot reports gains over Kimi K3 on its own tests. On Moonshot internal evaluations, long-horizon coding improved from 55.95 percent to 61.2 percent on Kimi Code Bench 3.0, and the claim-level hallucination rate on Kimi Hallu Bench Hard dropped from 23.4 percent to 8.3 percent.

### Need Help Mapping Where a Model Fits Your Team?

At Layer3Labs, we build custom agents, enterprise integrations, and advisory engagements for business teams. Book a workflow audit to map where a model like Kimi K3.1 Pro fits.

Book a Free Audit
