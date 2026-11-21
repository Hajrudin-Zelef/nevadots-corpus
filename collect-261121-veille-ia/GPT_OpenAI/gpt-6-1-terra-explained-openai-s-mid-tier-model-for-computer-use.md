---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-explained-openai-s-mid-tier-model-for-computer-use
title: "GPT-6.1 Terra Explained: OpenAI's Mid-Tier Model for Computer Use"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-26"]
keywords: ["terra", "openai", "gpt-6", "tasks", "work", "model", "computer", "astra"]
source: https://dev.to/neuraitool/gpt-61-terra-explained-openais-mid-tier-model-for-computer-use-4dmf
source_anchor: ""
source_lines: [1, 39]
section: "GPT_OpenAI"
---
OpenAI released **GPT-6.1 Terra** on October 26, 2026 — a mid-tier model built for computer use: reading screens, clicking, typing, and moving between apps to finish multi-step tasks. It is the model OpenAI expects you to point at routine agent work, because the pricing is where the story is.

- **$1 per million input tokens**
- **$5 per million output tokens**
- **$0.05 per million cached input tokens**

That is 50% below the GPT-5.6 Terra it replaces ($2 in / $12 out) and exactly half of GPT-6.1 Sol's rates — roughly a tenth of flagship Astra per token. OpenAI also says the same plan allowance in ChatGPT Work and Codex covers more tasks with Terra than with its predecessor.

The model ID in the API is `gpt-6.1-terra`. It is available to Plus, Pro, Business, Enterprise, and Edu users in ChatGPT Work and Codex, on Amazon Bedrock, and — as of the same day — **generally available in GitHub Copilot** for everyday agentic coding and terminal work. Free and Go plans still get GPT-5.6 Terra for now, and Terra is not available in Chat itself (it lives in Work and Codex).

Every figure here is vendor-reported; independent benchmarks are not out yet:

- 
**Agents' Last Exam** (professional computer tasks): +15 percentage points over GPT-5.6 Terra at medium effort, at about a third of the cost. Matches GPT-6 Astra at a tenth of the per-token price. Finishes tasks 3x+ faster than Astra and Sol, per OpenAI.
- 
**Terminal-Bench 4.0** : 50.3% of tasks at max effort, up from 21.5% for the predecessor.
- 
**Factual reliability** : error rate on OpenAI's own error-inducing prompts fell from 53.6% to 16.7% at low effort (4.3% at max).
- 
**vs Claude Opus 5.5** : OpenAI claims 3x+ token efficiency at a quarter of the price per token — ~9x lower cost per task.

GitHub's own early testing found Terra solved noticeably more terminal and C# tasks than GPT-5.6 Terra and worked in focused, consolidated steps on well-scoped tasks.

The lineup is now four tiers: **Luna** (lightweight) < **Terra** (mid-range workhorse) < **Sol** (strong reasoning) < **Astra** (flagship). OpenAI's own guidance: Sol and Astra remain the pick for the hardest coding and reasoning. Terra's pitch is volume — back-office automation that never penciled out at Sol prices.

Three practical takeaways:

1. 
**Treat Terra as your agent fleet's default tier.** The economics finally work for running agents all day on routine screen-and-form work: reading invoices into accounting software, filling long forms, interpreting technical drawings.
2. 
**Cache-aware design matters.** At $0.05/M cached input, repeated workflows with shared context get dramatically cheaper — structure your agents accordingly.
3. 
**Wait for independent numbers before committing at scale.** The gains are large but self-reported. Prototype on Terra now, hold off on architectural bets until third-party benchmarks land.

Terra is the October release that makes agent economics boring in the best way: cheap enough to stop thinking about per-task cost.

*This is a condensed developer explainer. The full breakdown — including complete pricing, pros/cons, and who should switch — is on NeurAITool: GPT-6.1 Terra: OpenAI's Cheap AI That Uses Computers (2026).*

## Top comments (0)
