---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-tess-docs
title: "Claude Opus 5.5 - Tess Docs"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "claude", "tools", "more", "result", "review", "work", "decisions"]
source: https://docs.tess.im/en/claude-opus-5-5
source_anchor: ""
source_lines: [1, 48]
section: "Claude_Anthropic"
---
**agentic coding, knowledge work, and complex long-running tasks**. It combines performance comparable to Claude Fable 5.1 on most work with faster and more efficient operation than Opus 5. The most relevant improvement is not only solving difficult tasks. Opus 5.5 was designed to reach the result with fewer steps, fewer tokens, and less rework—while remaining clear enough for a person to follow, review, and approve the execution.

**40% less on typical workloads**, generates output more than

**30% faster**, and communicates decisions more directly.

**680,000-line code migration in less than one day**. Opus 5.5 puts the most important information first, reduces unnecessary explanations, and follows provided writing rules more closely. This matters in long sessions: decisions and changes are easier to review before moving to the next step. Anthropic reports stronger prompt-injection resistance and a lower tendency to take hard-to-reverse actions or exceed defined boundaries. These improvements reduce risk, but do not replace human approval for critical changes. In results published by Anthropic, Claude Opus 5.5 advances in agentic coding, computer use, and professional work. The results below mostly use adaptive thinking at Max effort; Terminal-Bench 4.0 used XHigh.

**Expected result:**an end-to-end implementation, executed validations, and documented decisions ready for review. Dispersed sources must be combined, numbers must be checked, and the recommendation must arrive in a decision-ready format. The model brings research, analysis, and clearer communication together in the same execution.

**Expected result:**consistent executive material with traceable calculations, evidence, and explicit gaps. In long-running operations, capability without control increases risk. Opus 5.5 improves self-verification and adherence to defined boundaries, making it more suitable for agents that execute many steps and tools.

**Expected result:**continuous, auditable execution that stops for approval whenever an action exceeds the authorized scope.

Choose work that justifies the model

Provide context and criteria

Define tools and approvals

Start at the default effort

Request a review before completion

**Input:**repositories, architecture, expected behavior, and validation commands.

**Tools:**code, terminal, tests, and technical documentation.

**Done when:**all repositories remain compatible, tests pass, and the rollback plan is documented.

**Input:**documents, spreadsheets, external sources, and a business question.

**Tools:**search, data analysis, and document generation.

**Done when:**numbers and citations are traceable and all three deliverables tell the same story.

**Input:**application, metrics, behavioral requirements, and test environment.

**Tools:**code, profiling, terminal, and test suite.

**Done when:**the improvement is measured, behavior is preserved, and the result can be reproduced.

- Adaptive thinking remains active; thinking tokens are billed as output.
- A 1M-token context does not replace information selection. Send only relevant content.
- At XHigh and Max, leave enough output room for thinking, tools, and subagents.
- Safeguards may reroute certain cybersecurity and biology tasks to other Claude models.
- Review code, numbers, citations, and files before publishing or running in production.
- Keep human approval for financial, legal, clinical, security, or other high-impact decisions.
