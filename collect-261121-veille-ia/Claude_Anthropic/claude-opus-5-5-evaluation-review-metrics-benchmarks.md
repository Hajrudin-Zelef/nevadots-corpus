---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-evaluation-review-metrics-benchmarks
title: "Claude Opus 5.5 | Evaluation Review & Metrics Benchmarks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "fell", "claude", "findings", "density", "blockers", "evaluation", "review"]
source: https://www.sonarsource.com/blog/claude-opus-5-5-an-evaluation/
source_anchor: ""
source_lines: [1, 7]
section: "Claude_Anthropic"
---
## TLDR overview

- Claude Opus 5.5 posts an 87.7% pass result across the 544 HumanEval and MBPP tasks with executable tests, against 88.6% for Opus 5. The two releases are within one percentage point of each other.
- Opus 5.5 writes 27.5% less code for the same benchmark, 664,890 lines against Opus 5's 916,813, and uses 40% fewer tokens to do it.
- Total findings fell 42%, from 18,814 to 10,941. Code smell density fell 21% and vulnerability density fell 9%.
- BLOCKER (the most severe) findings came down across the board. Reliability blockers fell 41%, security blockers fell 53%, and maintainability blockers fell 20%.
- Bug density rose 12%, from 576 to 644 per mLOC, and concurrency findings rose 44%.
