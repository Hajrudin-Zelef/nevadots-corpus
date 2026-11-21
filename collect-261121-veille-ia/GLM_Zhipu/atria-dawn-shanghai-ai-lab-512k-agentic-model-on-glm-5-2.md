---
id: collect-261121-veille-ia/glm-zhipu/atria-dawn-shanghai-ai-lab-512k-agentic-model-on-glm-5-2
title: "Atria Dawn: Shanghai AI Lab 512K Agentic Model on GLM-5.2"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["atria", "dawn", "model", "preview", "shanghai", "agentic", "glm-5", "listed"]
source: https://tpsreport.news/news/atria-dawn-shanghai-ai-lab-512k-agentic-model
source_anchor: ""
source_lines: [1, 55]
section: "GLM_Zhipu"
---
# Shanghai AI Lab releases Atria Dawn, a 512K-context agentic model built on 744B-parameter GLM-5.2

Shanghai Artificial Intelligence Laboratory has released Atria Dawn, an agentic model with a 512K-token context window, built on the 744B-parameter mixture-of-experts GLM-5.2 foundation model. The model is published under the `internlm` organization on Hugging Face, with weights also listed on ModelScope.

## What Atria Dawn targets

According to the lab, Atria Dawn is aimed at research and engineering scenarios that need continuous environmental understanding, tool use and multi-step task completion. The lab says it supports the full loop of problem analysis, solution design, tool use, code implementation, experiment execution, result analysis and failure recovery. It describes four focus areas:

- **Discovery:** retrieving and organizing evidence, deep research, and turning research questions into experimental plans.
- **Creation:** building software, interactive applications, games, data visualizations and machine learning systems.
- **Delivery:** turning documents, data and design requirements into reports, presentations and other structured deliverables.
- **Cybersecurity:** analyzing issues, validating vulnerabilities, applying fixes and re-validating in authorized environments.

## Releases and specs

Four checkpoints are listed, all Instruct models:

| Model | Context | 
|---|---|
| Atria Dawn (full release) | 512K | 
| Atria Dawn FP8 (full release) | 512K | 
| Atria Dawn Preview | 256K | 
| Atria Dawn Preview FP8 | 256K | 

Atria Dawn accepts text input only. Per the model page, the endpoint rejects image input with a 400 error. Pricing is not yet disclosed.

## Benchmarks (lab-reported)

The lab compared Atria Dawn and Atria Dawn Preview against DeepSeek V4 Pro 0813, KIMI K3, Qwen 3.8 Max, GLM 5.3, GPT 5.6 sol and Claude Opus 5. These figures come from the lab's own table, which it says matches its official website.

Selected Atria Dawn scores (Preview in parentheses):

- AutomationBench: 54.7 (53.8)
- CyberGym: 86.8 (86.5)
- SkillsBench: 67.8 (66.4)
- WideSearch: 84.6 (81.9)
- MLE-bench Lite: 88.2 (86.2)
- PaperBench: 89.6 (87.0)
- BrowseComp: 91.3 (92.5)
- DeepSearchQA: 96.5 (96.0)
- BFCL V4: 78.4 (77.0)
- Terminal-Bench 2.1: 81.0 (78.3)
- SWE-bench Pro: 61.7 (59.6)

On several coding benchmarks the table shows other models scoring higher. Claude Opus 5 is listed at 90.2 on Terminal-Bench 2.1, 76.5 on SWE-bench Pro, 79.9 on NL2Repo-Bench and 41.0 on Harbor-Index, against Atria Dawn's 81.0, 61.7, 50.0 and 17.3. Atria Dawn is listed at 49.3 on DeepSWE, with no Preview result. Other cells in the table are marked unavailable.

## Deployment and access

Local deployment requires SGLang v0.5.13.post1 or later, or vLLM v0.23.0 or later. Hosted access is through separate international and China API consoles. Requests use `Atria-Dawn` as the model value.

The OpenAI-compatible base URL is `https://discovery-api.intern-ai.org.cn/v1`. The same host without `/v1` serves Anthropic-compatible clients and SystemOne. The documentation includes a Codex configuration that uses the Responses API and requires Codex CLI 0.154.0 or later. It also covers a Claude Code setup with a PreToolUse hook that blocks the Read tool on PDF and image files. For Codex, the guide sets `input_modalities` to text and `context_window` to 512000 so the client budgets the prompt correctly and does not send images.

## What this means

Atria Dawn pairs a 744B-parameter MoE base with a 512K window and ships FP8 variants for both the full and Preview checkpoints. The documented support for SGLang, vLLM, OpenAI-compatible and Anthropic-compatible endpoints, Codex and Claude Code means existing agent tooling can be pointed at it with configuration changes. The text-only limit matters for workflows that depend on screenshots or PDFs, which the documentation tells users to handle by pasting text. On the lab's own numbers, the model's strongest showing is in search, research and security tasks, while its coding results are mixed against the listed comparison models. Independent verification of these scores and pricing details are still outstanding.
