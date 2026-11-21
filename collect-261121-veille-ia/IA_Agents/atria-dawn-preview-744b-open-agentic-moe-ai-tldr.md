---
id: collect-261121-veille-ia/ia-agents/atria-dawn-preview-744b-open-agentic-moe-ai-tldr
title: "Atria Dawn Preview: 744B Open Agentic MoE | AI/TLDR"
domain: ia-agents
role: reference
task: article
actors: []
dates: ["2026-09-11"]
keywords: ["model", "dawn", "preview", "atria", "agentic", "published", "card", "open"]
source: https://ai-tldr.dev/models/atria-dawn-preview/
source_anchor: ""
source_lines: [1, 102]
section: "IA_Agents"
---
## Overview

Atria Dawn Preview is an agentic model from the Shanghai Artificial Intelligence Laboratory, published on 11 September 2026 under an MIT licence. The model card describes it as a preview release of a new-generation agentic model built on the 744B-parameter MoE GLM-5.2 foundation model, aimed at research and engineering work that demands continuous environmental understanding, tool use and multi-step task completion.

Its training story is the argument. The accompanying paper, "Atria Dawn: The Dawn of Agentic Superintelligence" (arXiv 2609.15818, submitted 14 September 2026), describes a Verifiable Experience Pipeline that connects tool-mediated interactions to executable environments and externally verified outcomes â the model is graded on whether a run actually produced a checkable result, not on whether the transcript looked right. The abstract reports that across 16 benchmarks spanning real-world research, engineering and digital work, Atria Dawn Preview is competitive with frontier agents and holds the highest reported score on five of them.

The model card organises capability into four dimensions: Discovery (retrieving and organising evidence, deep research, turning questions into executable experimental plans), Creation (software, interactive applications, games, data visualisations, machine-learning systems), Delivery (turning documents, data and design requirements into reports and presentations) and Cybersecurity (analysing issues, validating vulnerabilities, applying fixes and re-validating in authorised environments).

Two checkpoints are published â the instruct model and an FP8-quantised variant â both with a 256K context, on Hugging Face and ModelScope. For local serving the card specifies SGLang v0.5.13.post1 or newer and vLLM v0.23.0 or newer, pointing at the GLM-5.2 cookbook and recipe pages. Hosted access runs through api.atria-asi.ai internationally and through discovery.intern-ai.org.cn in China. One practical note from the Codex setup instructions: the endpoint takes text input only and rejects images with "400 Atria-Dawn-Preview is not a multimodal model".

| Released | 2026-09-11 | 
|---|---|
| License | MIT | 
| Weights | Open weights | 
| Parameters | 744B Mixture-of-Experts (built on the GLM-5.2 foundation model) | 
| Context | 256K | 
| Architecture | Mixture-of-Experts with DeepSeek sparse attention (Hugging Face model type glm_moe_dsa) | 
| Modalities | Text | 
| Status | Preview â instruct and FP8 checkpoints published on Hugging Face and ModelScope, with hosted APIs for international and China regions | 

## Benchmarks

Atria Dawn Preview against named peers, as published on the model card

| Benchmark | Atria Dawn Preview | DeepSeek V4 Pro 0813 | Kimi K3 | Qwen 3.8 Max | GLM 5.3 | GPT-5.6 Sol | Claude Opus 5 | 
|---|---|---|---|---|---|---|---|
| DeepSearchQA | 96 | â | 95.9 | â | 94.7 | 93.2 | â | 
| BrowseComp | 92.5 | 83.4 | 91.2 | â | â | 92.2 | 90.8 | 
| WideSearch | 81.9 | â | 79.6 | 81.9 | 82.7 | 83.3 | â | 
| DeepResearch Bench II | 51.1 | 46.6 | 51.3 | 49.2 | 52.7 | 50.7 | 54.1 | 
| MLE-bench Lite | 86.2 | 86.8 | 85.8 | 81.3 | 80.8 | 88.9 | 88 | 
| SWE-bench Pro | 59.6 | 58.3 | 61.6 | 65.1 | 60.3 | 61.4 | 74.7 | 
| Terminal-Bench 2.1 | 78.3 | 78.7 | â | 89.3 | 85.4 | 85.1 | 90.2 | 
| BFCL v4 | 77 | 71.4 | 69.1 | â | 74.1 | â | â | 
| AutomationBench | 53.8 | 41.7 | 45.9 | 49.7 | 49.2 | 45.7 | 49.4 | 
| SkillsBench | 66.4 | 65 | 51.9 | 66.7 | 63.3 | 62.5 | 63.7 | 
| ÏÂ³-Bench Banking | 41.2 | 44.3 | 37.1 | 55.2 | 40.2 | 46.9 | 48.7 | 
| Workspace-Bench | 65 | 55.7 | 60.6 | 63.9 | 63.9 | 56 | 65.8 | 
| Workspace-Bench-Lite | 68.2 | 58.1 | 65.8 | 67.4 | 67.7 | 60.5 | 70.1 | 
| GDPval | 1583 | 1517 | 1611 | 1722 | 1667 | 1682 | 1768 | 
| JobBench | 50.3 | 54.1 | 54.3 | 52.7 | 58.2 | 45.4 | 68 | 
| CyberGym | 86.5 | 83.3 | 78.7 | 73.8 | 84.5 | 83.6 | â | 

### This model's scores

1. DeepSearchQA96%
2. BrowseComp92.5%
3. CyberGym86.5%
4. MLE-bench Lite86.2%
5. WideSearch81.9%
6. Terminal-Bench 2.178.3%
7. BFCL v477%
8. Workspace-Bench-Lite68.2%
9. SkillsBench66.4%
10. SWE-bench Pro59.6%
11. AutomationBench53.8%
12. DeepResearch Bench II51.1%

Scores on a 0â100 scale (25-point gridlines); higher is better. Each benchmark links to its published source.

## Strengths

- MIT licence on a 744B-parameter MoE â both the instruct and FP8 checkpoints are downloadable and self-hostable
- Highest published score in its own comparison on DeepSearchQA (96.0), BrowseComp (92.5), BFCL v4 (77.0), AutomationBench (53.8) and CyberGym (86.5)
- Trained through a Verifiable Experience Pipeline that grades runs on externally verified outcomes rather than transcript plausibility
- 256K context on both checkpoints, with documented SGLang and vLLM deployment paths
- Hosted endpoints for both international and China regions, plus a documented Codex custom-provider configuration

## Best for

- Deep-research and evidence-gathering agents that need to turn a question into an executable plan
- Long-running engineering agents that write code, run experiments and recover from failures
- Authorised security work: analysing issues, validating vulnerabilities and re-checking fixes
- Self-hosted agent stacks where an MIT licence and downloadable weights matter more than a hosted frontier API

## How to access

## Atria (agentic) â every version

The full lineage of the Atria (agentic) line, newest first. Every version has its own page â click any to compare specs, benchmarks and pricing.

## FAQ

## What is Atria Dawn Preview built on?

The 744B-parameter MoE GLM-5.2 foundation model, according to the model card. Hugging Face reports the architecture as glm_moe_dsa, and the deployment guides point at the GLM-5.2 SGLang cookbook and vLLM recipe pages.

## Is it multimodal?

No. The Codex setup section in the README states the endpoint accepts text input only, and rejects attached images with the error "400 Atria-Dawn-Preview is not a multimodal model".

## What licence are the weights under, and which checkpoints exist?

MIT. Two checkpoints are published on both Hugging Face and ModelScope: the instruct model (Atria-Dawn-Preview) and an FP8-quantised variant (Atria-Dawn-Preview-FP8), each with a 256K context.

## How do I run it locally?

The model card lists two supported frameworks with minimum versions: SGLang v0.5.13.post1 or newer, and vLLM v0.23.0 or newer, each with a GLM-5.2 deployment guide. Hosted access is available at api.atria-asi.ai internationally and through discovery.intern-ai.org.cn in China.

## What does the Verifiable Experience Pipeline mean?

It is the training approach described in the paper: tool-mediated interactions are connected to executable environments and to externally verified outcomes, so the reward signal comes from whether a run produced a checkable result. The paper reports the model is competitive with frontier agents across 16 benchmarks and holds the highest reported score on five.
