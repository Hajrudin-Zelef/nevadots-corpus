---
id: collect-261121-veille-ia/atria-dawn/shanghai-ai-lab-posts-full-atria-dawn-weights-under-mit-with-a-512k-context-sig-advisory-news
title: "Shanghai AI Lab posts full Atria Dawn weights under MIT, with a 512K context | Sig Advisory News"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["model", "card", "shanghai", "lab", "posts", "full", "atria", "dawn"]
source: https://sigadvisory.co/news/atria-dawn-open-weights-mit
source_anchor: ""
source_lines: [1, 5]
section: "Atria_Dawn"
---
The model is text only. The lab's own table (self-reported) has it ahead on agent and tool benchmarks like AutomationBench (54.7) and BFCL V4 (78.4), but well behind Claude Opus 5 on coding: 61.7 vs 76.5 on SWE-bench Pro and 50.0 vs 79.9 on NL2Repo.

You can run it locally with SGLang v0.5.13.post1 or later, or vLLM v0.23.0 or later. There's also a hosted API with OpenAI- and Anthropic-compatible endpoints. The model card includes setup for Codex CLI, Claude Code and Kimi Code. Hardware requirements aren't published, and at 744B parameters this is a multi-GPU server job, not a laptop one.

One gotcha from the card: Codex assumes every model takes images, so pasting a screenshot gets a 400 error. The card's fix is a model catalog file with input_modalities set to text.
