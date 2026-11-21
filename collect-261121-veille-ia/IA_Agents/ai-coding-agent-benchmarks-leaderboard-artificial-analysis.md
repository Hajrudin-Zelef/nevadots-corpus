---
id: collect-261121-veille-ia/ia-agents/ai-coding-agent-benchmarks-leaderboard-artificial-analysis
title: "AI Coding Agent Benchmarks & Leaderboard | Artificial Analysis"
domain: ia-agents
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "coding", "artificial", "analysis", "index", "tasks", "task", "across"]
source: https://artificialanalysis.ai/agents/coding-agents
source_anchor: ""
source_lines: [1, 75]
section: "IA_Agents"
---
# Artificial Analysis Coding Agent Benchmarks

We measure real-world performance of coding agents on software engineering tasks, including cost, token usage, and execution time. We compare how performance changes across agents, models, and execution settings.

## Artificial Analysis Coding Agent Index

Composite index of 3 benchmarks:

- FrontierCode-AA 2.0Software engineering tasks, 132 tasks
- Terminal-Bench 4.1Agentic terminal use, 66 tasks
- SWE-Atlas-QnA-AATechnical Q&A, 124 tasks

## Performance

Performance across the Artificial Analysis Coding Agent Index.

### Artificial Analysis Coding Agent Index

### Artificial Analysis Coding Agent Index vs. Cost per Task

## Token Usage

Token consumption across the Artificial Analysis Coding Agent Index, including total usage, token mix, efficiency, and per-benchmark breakdowns.

### Token Usage per Task

### Artificial Analysis Coding Agent Index vs. Total Tokens

## Cost

Cost across the Artificial Analysis Coding Agent Index based on current per-token API pricing, including cache write pricing and cache discounts where available. Many users will access coding agent harnesses through subscription plan offerings rather than pay-per-token.

### Cost per Task

## Execution Time

Active agent runtime across the Artificial Analysis Coding Agent Index.

### Time per Task

### Artificial Analysis Coding Agent Index vs. Execution Time

How often a provider or model refuses to proceed with a task on safety grounds across the Artificial Analysis Coding Agent Index.

### Safety Refusal Rate

Share of task attempts that would otherwise pass but are scored zero following an integrity violation.

### Reward Hacking Rate: FrontierCode-AA 2.0

## Harness Comparison

Coming soon

## Frequently Asked Questions

The Artificial Analysis Coding Agent Index is our composite score for coding-agent performance across the public benchmark suite on this page. It combines FrontierCode-AA 2.0, Terminal-Bench 4.1, and SWE-Atlas-QnA-AA to capture implementation, terminal workflow, repository-understanding, and broader software-engineering performance in a single headline metric.

The current public index includes FrontierCode-AA 2.0, Terminal-Bench 4.1, and SWE-Atlas-QnA-AA. These benchmarks are combined because they stress different parts of the coding-agent workflow rather than repeating the same task format.

The public benchmark suite mixes several software engineering task styles. Some tasks are Q&A and repository-understanding tasks that focus on reading a codebase, understanding architecture or behavior, and producing a correct technical answer. Some are implementation and bug-fix tasks that require code changes and are closer to the classic make-a-patch-that-works framing. Some are terminal workflow tasks that test whether the agent can navigate a shell-driven environment, execute tools correctly, and complete a multi-step command-line workflow. Terminal-Bench 4.1 and SWE-Atlas-QnA-AA score each attempt pass or fail; FrontierCode-AA 2.0 scores each attempt 0 or between 70% and 100%.

Q&A-style tasks emphasize repository understanding, code reading, tracing behavior, and producing a correct technical explanation. Implementation-style tasks are closer to shipping a working change: the agent has to understand the task, navigate the repository, edit files correctly, and satisfy an evaluator or test-based outcome under execution constraints. Those are related capabilities, but they are not identical. An agent can be strong at repository reasoning and still be weaker at reliable patch execution, or vice versa, which is one reason the composite index should be interpreted alongside the per-benchmark chart.

The benchmark page reports the task-normalized average score. For each benchmark, we first average the three evaluated attempts for each task, then average those task-level scores so every task has equal weight. Terminal-Bench 4.1 and SWE-Atlas-QnA-AA score each attempt pass or fail. FrontierCode-AA 2.0 grades each attempt against blocking and non-blocking checks: an attempt that fails any blocking check scores 0, and one that passes them all scores between 70% and 100%, higher when it also meets the non-blocking checks. An attempt can complete cleanly and still score zero when it does not satisfy its verifier. SWE-Atlas-QnA-AA binary pass/fail scoring is aligned with Scale AI's Task Resolve Rate methodology. View SWE-Atlas-QnA-AA grading on Hugging Face

The index is computed from FrontierCode-AA 2.0, Terminal-Bench 4.1, and SWE-Atlas-QnA-AA. For the current Artificial Analysis Coding Agent Index, the public methodology is a simple average across those benchmark scores. Benchmark methodology can evolve as coverage improves, so comparability is best interpreted within the published benchmark suite and its current component set rather than as a timeless absolute score.

Execution time on this page refers to average wall-clock task runtime per task, not just raw model latency. It is meant to reflect the user-facing time cost of running the whole agent workflow. That includes time spent reasoning, issuing tool calls, reading and writing files, executing shell steps, and waiting on model responses. So an agent can have a fast underlying model and still be slower overall if its workflow is longer or more tool-heavy.

Token usage is the average observed token consumption per task across the benchmark suite. On this page we break it out into input, cache, and output tokens. Input tokens are the tokens sent into the model, including prompts, instructions, tool context, and task context. Cache tokens are prompt tokens reused through prompt caching when the provider exposes that telemetry. Output tokens are tokens generated by the model in its response. Token usage matters because it often drives cost and can also indicate how much context an agent consumes to get work done, but token efficiency and cost are not identical because providers price token categories differently and caching can materially change the bill.

A higher index score means stronger performance across the included benchmark mix, but it does not mean the agent is best for every workflow. The index is a balance across benchmark quality, not a direct measure of your specific latency, cost, tooling, or task-type priorities. Real-world choice still depends on whether your workflow looks more like repository Q&A, patching, or terminal execution, and on practical constraints such as IDE integration, model availability, and reliability.

These benchmarks measure coding-agent performance across repositories, tools, multi-step workflows, and evaluator-based outcomes. Results on this page reflect specific evaluated agent variants, not just generic product names: model choice, settings, and execution configuration can materially change outcomes, which is why a single agent family may appear in multiple variants in the results. For more background on benchmark runs, task-level scoring, and methodology, see the coding-agents benchmarking methodology page. View the coding-agents benchmarking methodology
