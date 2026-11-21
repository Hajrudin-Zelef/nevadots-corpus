---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-mai-cyber-2-launch-results-and-rollout-neoteo
title: "Microsoft MAI Cyber 2 launch: results and rollout | NeoTeo"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "cyber", "flash", "results", "model", "reports", "says", "reasoning"]
source: https://neoteo.com/en/microsoft-mai-cyber-2-flash-launch-results-and-rollout
source_anchor: ""
source_lines: [1, 27]
section: "Copilot_Microsoft"
---
Microsoft announced MAI Cyber 2 Flash at Ignite on November 17, 2026, describing it as a cybersecurity model for finding vulnerabilities, recommending fixes and investigating threats. Microsoft reports results on two security benchmarks and says eligible Codename MDASH customers are scheduled to receive the model starting in mid-December.

**Practical takeaway:** Compare each score with its benchmark, task set and reasoning budget; the conditions differ between Microsoft’s two results.

## What results did Microsoft report?

Microsoft reports a 92.3% task-success rate for MAI Cyber 2 Flash on CyberGym-E2E, compared with 91.5% for GPT-6 Astra. Microsoft says MAI Cyber 2 Flash used half the reasoning budget. On WebExploitBench, Microsoft reports 40.4% for MAI Cyber 2 Flash and 38.4% for GPT-5.6 Cyber at the same reasoning budget. These are Microsoft’s own evaluation results, as described in its MAI Cyber 2 Flash announcement.

| Benchmark | MAI Cyber 2 Flash | Comparison model | Conditions Microsoft reports | 
| CyberGym-E2E | 92.3% task success | GPT-6 Astra: 91.5% | MAI Cyber 2 Flash used half the reasoning budget; results cover 813 of 905 tasks | 
| WebExploitBench | 40.4% | GPT-5.6 Cyber: 38.4% | Same reasoning budget | 

## What was included in the CyberGym-E2E comparison?

Microsoft says it excluded the same 92 of 905 tasks for every model because some benchmark tasks used open-source projects and vulnerability reports that overlapped with its training data. The reported comparison covers the remaining 813 tasks. Microsoft also says it did not use the benchmark’s reference solutions for training.

Microsoft describes the CyberGym-E2E cost comparison as an offline cost-per-task calculation using list prices for input, cached-input and output tokens. It reports that MAI Cyber 2 Flash’s cost per task was less than 2% of GPT-6 Astra’s in that evaluation. That figure describes the benchmark calculation, not customer spending.

## What is the model designed to do?

Microsoft positions MAI Cyber 2 Flash for vulnerability discovery, fix recommendations and threat investigation. It also states that the model has a **one-million-token context window**, four times the window of MAI Cyber 1.1 Flash.

For context on the broader Ignite announcement, NeoTeo previously covered Microsoft’s specialized MAI models and coding-cost comparisons.

## When is access planned?

Microsoft says MAI Cyber 2 Flash is planned exclusively for Microsoft security products. Eligible Codename MDASH customers are scheduled to receive it automatically in their MAI profile starting in mid-December 2026.
