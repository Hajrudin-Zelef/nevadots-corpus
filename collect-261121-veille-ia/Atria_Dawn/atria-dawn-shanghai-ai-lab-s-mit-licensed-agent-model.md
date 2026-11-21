---
id: collect-261121-veille-ia/atria-dawn/atria-dawn-shanghai-ai-lab-s-mit-licensed-agent-model
title: "Atria Dawn: Shanghai AI Lab's MIT-licensed agent model"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["atria", "dawn", "model", "agent", "shanghai", "code", "versus", "lab"]
source: https://www.neuraldaily.news/en/a/atria-dawn-shanghai-ai-lab-open-weight-agent-model
source_anchor: ""
source_lines: [1, 26]
section: "Atria_Dawn"
---
## What was released?

On October 23, Shanghai Artificial Intelligence Laboratory uploaded the full release of Atria Dawn to Hugging Face (Hugging Face), along with an FP8-quantized version. The September Atria Dawn Preview had 256K context; the full release supports 512K tokens.

- Base: the 744-billion-parameter GLM-5.2 MoE model.
- License: code and weights under MIT, so commercial use is unrestricted.
- Text only: the API rejects image or PDF input, and the model card includes setup guides for Codex, Claude Code and Kimi Code that strip such inputs.
- Deployment: SGLang v0.5.13.post1+ or vLLM v0.23.0+, or a hosted API compatible with OpenAI and Anthropic clients.

## What it's for

The lab targets research and engineering agent work: turning research questions into executable experiment plans, building software, producing reports and presentations from documents, and validating and fixing vulnerabilities in authorized environments. The goal is to drive tasks to verifiable, reproducible results.

## What the numbers show

In the lab's own table, Atria Dawn leads on AutomationBench (54.7), CyberGym (86.8) and WideSearch (84.6), ahead of Claude Opus 5, GPT 5.6 sol and Kimi K3. In coding, though, the gap is clear:

- Terminal-Bench 2.1: 81.0 versus 90.2 for Opus 5.
- SWE-bench Pro: 61.7 versus 76.5.
- NL2Repo-Bench: 50.0 versus 79.9.

These are the developer's own results; no independent evaluation is available yet.

## Why it matters

Few models this large ship under MIT, a real advantage for commercial use compared with restrictive community licenses. Based on the numbers, it's not the first pick for a coding agent, but it may be worth testing for search, automation and office tasks where self-hosting or licensing matters.
